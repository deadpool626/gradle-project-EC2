# gradle-project-EC2
**Part 1: Intro**

In this project, our goal is to set up Gradle on an Ubuntu machine and build an application. The code for the application will be using the default code provided by Gradle, making this project straightforward and easy to complete, taking only 15-20 minutes with a stable internet connection.

Before we dive in, it's important to have an AWS (Amazon Web Services) account. If you already have an Ubuntu machine ready, you can skip this section. However, if you don't have an AWS account, please follow these steps:
1. Set up an AWS account, log in, and navigate to the dashboard.
2. Find and click on "EC2 Instances."

Now, let's create a new instance:
1. Click on "Launch Instance."
2. A form will appear. Fill in the following details:
    - Name: "gradle-project"
    - OS Image: "Ubuntu"
    - Amazon Machine Image: "Amazon Linux 2023 AMI"
    - Instance Type: "t2.micro"
    - Select the default Key Pair.
3. Click on "Launch Instance."
4. Return to the instances section and wait until the Instance State shows as "Running," and Status Checks pass.
5. Select the "gradle-project" instance and click on "Connect."
6. Keep the connection settings as default and proceed. A new tab will open, displaying the CLI of our instance.

Congratulations! You've successfully launched a simple EC2 Ubuntu instance, which we will use for our project.
---
**Part 2: Setting Up The Things**

**Let's Get Started with the Real Work!**

Before you, a wide black screen awaits your command in the CLI.

**Step 1: Update Your System**

To begin, let's ensure that our Ubuntu system is up-to-date. Use the following command:

```bash
sudo apt-get update
```

Once our system is up to date, we can proceed to install Gradle. You have the option to refer to [https://gradle.org/install/](https://gradle.org/install/) or follow the steps below, which I recommend.

**Step 2: Install JDK and Download Gradle Package**

First, we need to install Java as it's a prerequisite for Gradle. Enter the following command:

```bash
sudo apt install default-jdk -y
```

The '-y' option automatically confirms 'yes' wherever the installer prompts. To verify that Java is installed, run:

```bash
java --version
```

You should see the installed JDK version. Next, download the 'Binary-Only' flavor of Gradle, which is smaller in size and contains everything required for your project:

```bash
wget -c https://services.gradle.org/distributions/gradle-8.4-bin.zip -P /tmp
```

The '-P' flag specifies the download path, which is '/tmp' in our case. You can confirm the downloaded zip file by running:

```bash
ls /tmp
```

You should see a zip file with the Gradle name and version.

**Step 3: Unzip the Package**

Unzip the downloaded package using:

```bash
sudo unzip -d /opt/gradle /tmp/gradle-8.4-bin.zip
```

If you encounter an 'Unzip Command Not Found' error, you can resolve it by installing unzip (use `sudo apt-get install zip unzip`) and then rerun the previous command. You can verify that the files are extracted in the specified path by running:

```bash
ls /opt/gradle/
```

**Step 4: Set Up Environment Variables**

Now, let's set up environment variables for Gradle using the Vim editor:

```bash
sudo vim /etc/profile.d/gradle.sh
```

After running this command, the 'gradle.sh' file will open in the Vim editor. Press "i" to enter input mode and add the following two lines, which are the environment variables we need to set:

```bash
export GRADLE_HOME=/opt/gradle/gradle-8.4
export PATH=${GRADLE_HOME}/bin:${PATH}
```

To save and exit the file, press `Esc`, then type `:wq` and press `Enter`. You can verify the content of the file using:

```bash
cat /etc/profile.d/gradle.sh
```

**Step 5: Make the File Executable**

Change the permission settings of the 'gradle.sh' file to make it executable:

```bash
sudo chmod +x /etc/profile.d/gradle.sh
```

**Step 6: Final Steps**

Finally, load the environment variables:

```bash
source /etc/profile.d/gradle.sh
```

To verify the installation, check Gradle's version:

```bash
gradle --version
```

Congratulations! You've completed the Gradle installation. It's been quite straightforward, hasn't it?

---

**Part 3: Let's Dive into the Real Work!**

Now that we're all set up, it's time to get down to business.

**Step 1: Create a Project Directory**

Start by creating a project directory for our Gradle project and navigate into it:

```bash
mkdir gradle-java-project
cd gradle-java-project/
```

**Step 2: Initialize the Gradle Project**

Gradle comes with a built-in task called 'init' that initializes a new Gradle project in an empty folder. This task also creates a Gradle wrapper script, 'gradlew.' From within the project directory ('gradle-java-project'), run the 'init' task using the following command:

```bash
gradle init
```

When prompted, select the following options:
- Project type: 2 (application)
- Implementation language: 3 (Java)
- DSL for writing build scripts: Press Enter to use the default values for the other questions.

The 'init' task generates the project with a specific structure. To visualize this structure, you can install the 'Tree' library in your system with the following commands:

```bash
sudo apt-get install tree
tree --version
```

Now, run the 'tree' command in the 'gradle-java-project' directory to see the file structure:

```bash
tree
```

You'll observe a structured file system:

```
.
├── app
│   ├── build.gradle.kts
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── gradle
│       │   │       └── java
│       │   │           └── project
│       │   │               └── App.java
│       │   └── resources
│       └── test
│           ├── java
│           │   └── gradle
│           │       └── java
│           │           └── project
│           │               └── AppTest.java
│           └── resources
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle.kts
```
Let's take a overview of it:

```
.
├── app
│   ├── build.gradle.kts (Build script of the app project)
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── gradle
│       │   │       └── java (Default Java source folder)
│       │   │           └── project
│       │   │               └── App.java
│       │   └── resources
│       └── test
│           ├── java
│           │   └── gradle
│           │       └── java
│           │           └── project (Default Java test source folder)
│           │               └── AppTest.java
│           └── resources
├── gradle (Generated folder for wrapper files)
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew (Gradle wrapper start scripts)
├── gradlew.bat
└── settings.gradle.kts (Settings file to define build name and subprojects)
```

**Step 3: Modify the Source Code**

To make a small change to our project's source code, open the 'App.java' file in Vim:

```bash
vim app/src/main/java/gradle/java/project/App.java
```

Press "i" to enter insert mode and add the following line within the `main()` function:

```java
System.out.println("***\nThis is my small and quick project on Gradle!\n***");
```

Save and exit the file by pressing 'Esc', then typing `:wq`.

**Step 4: Run the Application**

Now, let's run our application with the following command:

```bash
./gradlew run
```

(Note: The first time you run the wrapper script, 'gradlew,' there may be a delay while that version of Gradle is downloaded and stored locally in your '~/.gradle/wrapper/dists' folder.)

You should see an output similar to this:

```
Starting a Gradle Daemon, 1 busy Daemon could not be reused, use --status for details
Path for java installation '/usr/lib/jvm/openjdk-11' (Common Linux Locations) does not contain a java executable

> Task :app:run
Hello World!
***
This is my small and quick project on Gradle!
***

BUILD SUCCESSFUL in 20s
2 actionable tasks: 2 executed
```

**Step 5: Bundle the Application**

To bundle our application, run:

```bash
./gradlew build
```

If you run a full build, Gradle will produce the archive in two formats: 'app/build/distributions/app.tar' and 'app/build/distributions/app.zip.'

This way, we've successfully created, modified, and bundled our small application.

---


   
