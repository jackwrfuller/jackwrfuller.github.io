
+++
title = "Turning JavaFX Into an Executable in 2023"
date = "2023-05-14"
+++

In my university course this semester, I was tasked with creating a JavaFX game. As this was my first big project, this was a difficult but rewarding challenge where I learned so much. While I go into more detail about the project [here](https://jackwrfuller.github.io/projects/blue-lagoon), I wanted to share some of the smaller things I learned while embarking on this journey so that one day someone out there will not have to spend the same ridiculous hours troubleshooting and furiously googling that I did.  

As it turned out, one of the most surprising tasks in terms of its difficulty was turning the JavaFX game first into a jar file, and then an executable. Early on in the project, I thought it would be cool to turn the game into a standalone application - you know, like a proper game. Since I am developing on a Windows machine, this was a .exe file. I thought to myself that it would be a matter of pressing some button in IntelliJ (my IDE). Oh boy, how wrong I was. So, in the hope to save some poor soul the tribulations I did, I want to share how to turn a JavaFX project in IntelliJ into an exe file. 

# My set up
For the sake of reproducibility, here is my set up:
- Windows 10
- Java 17 (OpenJDK 17.0.6)
- JavaFX 17.0.6
- IntelliJ IDEA 2022.3.2 (Ultimate Edition)

There are many great tutorials for the installation of the above, so I wont go into it here. Furthermore, you will need [Launch4J](https://launch4j.sourceforge.net/), which is my solution for converting jar files to windows executables.

# From Java to Jar
You might think that building a jar file, especially in a modern IDE such as IntelliJ, would be trivial. Perhaps in a normal case without JavaFX, this would be the case. However, for whatever reason, JavaFX definitely throws a spanner in the works. First, navigate to `File` -> `Project Structure`



