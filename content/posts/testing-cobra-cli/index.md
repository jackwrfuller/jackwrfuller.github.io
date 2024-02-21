+++
title="Golang: Testing Cobra CLI applications with dependency injection"
weight=1
date=2024-02-21
+++

# Golang: Testing Cobra CLI applications with dependency injection

 When creating a command line interface (CLI) application, the Go programming language and the [Cobra CLI framework](https://github.com/spf13/cobra) are popular choices.
 I was recently introduced to this stack while helping develop the [GovCMS CLI application](https://github.com/govcms-tests/govcms-cli). 
 Like the good little engineer I am, I eventually investigated how I could go about actually testing what I had written.

Suppose we want a base command 'boom' which has two subcommands 'small' and 'big', i.e a user using our app has access to commands,

```shell
boom small
```
and
```shell
boom big
```

Our first iteration of this project might look something like this:

```go
package main

import "github.com/spf13/cobra"

var rootCmd = &cobra.Command{
	Use:   "boom",
	Short: "Explode things!",
}

var smallBoomCmd = &cobra.Command{
	Use:   "small",
	Short: "Small explosion",
	Run: func(cmd *cobra.Command, args []string) {
		// Do something
	},
}

var bigBoomCmd = &cobra.Command{
	Use:   "big",
	Short: "big explosion",
	Run: func(cmd *cobra.Command, args []string) {
		// Do something else
	},
}

func main() {
	rootCmd.AddCommand(smallBoomCmd)
	rootCmd.AddCommand(bigBoomCmd)

	rootCmd.Execute()
}
```

How do we go about testing this?

## Method 1: Don't

The simplest and probably most robust approach is to have the Cobra framework pass all the necessary to a helper function - one completely independent of Cobra - and test that instead.
This idea was introduced to me by @Elwinar on [this](https://stackoverflow.com/a/35827855) stackoverflow post. It looks like this:

```go
func main() {

	var rootCmd = &cobra.Command{
		Use:   "boom",
		Short: "Explode things!",
	}

	var smallBoomCmd = &cobra.Command{
		Use:   "small",
		Short: "Small explosion",
		Run: func(cmd *cobra.Command, args []string) {
			small(args...)
		},
	}

	var bigBoomCmd = &cobra.Command{
		Use:   "big",
		Short: "big explosion",
		Run: func(cmd *cobra.Command, args []string) {
			big(args...)
		},
	}

	rootCmd.AddCommand(smallBoomCmd)
	rootCmd.AddCommand(bigBoomCmd)

	rootCmd.Execute()
}

func small(args ...string) {
	// Do something
}

func big(args ...string) {
	// Do something else
}
```

You would then create tests for `small()` and `big()` in the standard Go fashion (If you need a refresher on this, [this](https://blog.jetbrains.com/go/2022/11/22/comprehensive-guide-to-testing-in-go/) might be helpful).
This method works and introduces a separation of concerns, usually considered an improving change to any code. I would imagine that if this is possible in your case, most would recommend this approach.

However, it might be the case you wish to test _Cobra itself_. Why? Perhaps the `Run` field function command be abstracted nicely into a single function, or maybe you wish to create integration tests of Cobra, rather than unit tests.
In any case, there is another approach.

## Method 2: Command factories with dependency injection
This method was born out of [this](https://gianarb.it/blog/golang-mockmania-cli-command-with-cobra) blog post by [Gianluca Arbezzano](https://github.com/gianarb).
When testing, we usually like to mock our dependencies in tests to ensure they run as fast as possible while testing exactly the code we wish to test.
Mocking requires being able to inject our dependencies, so we can use both the real and mocked versions at different times. Gianluca proposed a command factory that takes in dependencies like a constructor.

```go
package main

import "github.com/spf13/cobra"

func NewRootCmd( /*Dependencies inserted here */ ) *cobra.Command {
	rootCmd := &cobra.Command{
		Use:   "boom",
		Short: "Explode things!",
	}
	rootCmd.AddCommand(smallBoomCmd)
	rootCmd.AddCommand(bigBoomCmd)

	return rootCmd
}

var smallBoomCmd = &cobra.Command{
	Use:   "small",
	Short: "Small explosion",
	Run: func(cmd *cobra.Command, args []string) {
		// Do something
	},
}

var bigBoomCmd = &cobra.Command{
	Use:   "big",
	Short: "big explosion",
	Run: func(cmd *cobra.Command, args []string) {
		// Do something else
	},
}

func main() {
	rootCmd := NewRootCmd()
	rootCmd.Execute()
}
```

Note that the subcommands are added _inside_ the command factory `NewRootCmd()`. This was not something discussed in Gianluca's post.
We can then run a test using something like this:

```go
func Test_SmallCmd(t *testing.T) {
	cmd := NewRootCmd()
	cmd.SetArgs([]string{"small"})
	cmd.Execute()
	
	// assert something here
}
```

This method as written currently has two major drawbacks however: 
 - execution of commands has to be done explicitly which can make capturing of results tricky
 - all subcommands are defined globally which means different root commands created using the factory all use the same subcommand instances

The first issue is relatively minor and can improved by using some test execution fixture. The second issue was more subtle, and was causing some annoying bugs for me that took an extended debugging session to realise. 
 Fortunately, we can work around these two issues quite simply.

### Improving command execution
Ideally, we want some function that encapsulates command execution and provides us with the string output of running this command (string because, well, its a CLI app).
I can't remember where I was pointed to a solution for this, but someone suggested looking at the tests for Cobra itself. Sure enough, Steve Francia being the legend he is, provided the goods with `executeCommand(root *Command, args ...string )(string, error)` in [command_test.go](https://github.com/spf13/cobra/blob/bcfcff729ecdeb4072ef6f2a9c0b5e4ca1853206/command_test.go#L32).
Since this function is unexported, I had to bring it in-house by copying the following two functions into the project:
```go
func ExecuteCommand(root *cobra.Command, args ...string) (output string, err error) {
	_, output, err = ExecuteCommandC(root, args...)
	return output, err
}

func ExecuteCommandC(root *cobra.Command, args ...string) (c *cobra.Command, output string, err error) {
	buf := new(bytes.Buffer)
	root.SetOut(buf)
	root.SetErr(buf)
	root.SetArgs(args)

	c, err = root.ExecuteC()

	return c, buf.String(), err
}

```

These two functions can be modified and extended if needed, but for my purposes they were sufficient. With them, our tests now look something like this:

```go
func Test_SmallCmd(t *testing.T) {
	cmd := NewRootCmd()
	consoleOutput, err := ExecuteCommand(cmd, "small")

	// assert something about output/errors here
}
```

### Preventing singleton subcommands
As it stands, our `smallCmd` and `bigCmd` are singletons. This might not seem like a big deal, but it can lead to some unexpected and certainly undesirable results.
In my case, I was creating a subcommand that could take two flags, lets call them `radius` and `diameter`. I wanted users to be able to specify one, but not both. I.e

```shell
boom small --radius=100 // Allowed
```

```shell
boom small --diameter=200 // Allowed
```

```shell
boom small --radius=10 --diameter=50 // Not allowed
```

I created tests for each case, and each test passed. However an odd thing happened: when I ran all the test sequentially in a test suite, most of them failed! 
As it turned out, flags are attached to the subcommands, and by calling the subcommand with that flag, I was forever retaining the fact I had _changed_ that flag. After running the first two tests,
all commands would then fail, as it still thought both flags had been called.

The solution?

The subcommands need to be created with factories  as well so that there lifetime is synchronised with the root command. Given we already achieved this with the root command, it is not that difficult:

```go

func NewRootCmd( /*Dependencies inserted here */ ) *cobra.Command {
	rootCmd := &cobra.Command{
		Use:   "boom",
		Short: "Explode things!",
	}
	rootCmd.AddCommand(newSmallCmd())
	rootCmd.AddCommand(newBigCmd())

	return rootCmd
}

func newSmallCmd() *cobra.Command {
	smallCmd := &cobra.Command{
		Use:   "small",
		Short: "Small explosion",
		Run: func(cmd *cobra.Command, args []string) {
			// Do something
		},
	}
	// You can register flags, grandchild commands, etc here

	return smallCmd
}

func newBigCmd() *cobra.Command {
	bigCmd := &cobra.Command{
		Use:   "big",
		Short: "Big explosion",
		Run: func(cmd *cobra.Command, args []string) {
			// Do something
		},
	}
	// You can register flags, grandchild commands, etc here

	return bigCmd
}

func main() {
	rootCmd := NewRootCmd()
	rootCmd.Execute()
}
```

And that's it! I'm sure that there are  ways to even further improve this solution, so if you have any feel free to reach out.
