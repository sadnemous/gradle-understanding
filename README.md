### This note is comprised of following:
  1. https://www.youtube.com/watch?v=lEfbYkacy1I&
  2. https://www.youtube.com/watch?v=R6Z-Sxb837I&
  3. CoPilot Answers

### What is Gradle: 
 - gradle basically build automation tool which takes all the code in your project and packages it into a deployable uint that can run inthe tRagert environment.
 - build.gradle.kts is equivalent to pom.xml
 - gradle is less verbose than maven


### How do I setup a new project with the Gradle Wrapper?
 - to setup a new project to use Gradle Wrapper, you need to install first gradle
 - go to gradle.org/install
 - once installed add gradle bin dir to PATH
 - make empty dir `mkdir test_wrapper`
 - `cd test_wrapper`
 - `gradle wrapper`
 <br><img src="img/grdl.1.png" width=600>
 - `tree`
 <br><img src="img/grdl.2.png" width=400>
 - `gradle-wrapper.jar` - The gradle-wrapper.jar is used to download the correct version of Gradle specified in the gradle-wrapper.properties file when you run Gradle commands. This ensures that your project builds consistently with the same Gradle version, regardless of the version installed on the local machine.

 - `gradle-wrapper.properties` - the gradle-wrapper.properties file is used to specify the version of Gradle that should be used for the project. When you run Gradle commands, this file ensures that the correct version of Gradle is downloaded and used, providing consistency in the build process regardless of the Gradle version installed on the local machine.

 - `gradlew` - build script for Linux
 - `gradlew.bat` - build script for Windows
 
 ### Should I commit these files and folder to version control?
 These files and folders should be part of the repository to ensure that anyone who clones the repository can build the project with the exact same version of Gradle, regardless of the version installed on their local machine. This provides consistency and reliability in the build process. Specifically:

- `gradle-wrapper.jar`: Ensures the correct version of Gradle is downloaded.
- `gradle-wrapper.properties`: Specifies the version of Gradle to use.
- `gradlew` and `gradlew.bat`: Scripts to run Gradle commands on Linux and Windows, respectively.

Including these files in version control ensures that all developers and CI/CD systems use the same Gradle version, avoiding potential build issues caused by version discrepancies.


 ### How can upgrade to higher version
 ```bash
 ./gradlew --version
 Gradle 6.1.1

 ./gradlew wrapper --gradle-version 6.2.2
 ./gradlew --version
 ```
 <br><img src="img/grdl.3.png" width=800>
 
### Where does Gradle Wrapper store gradle?
```bash
cd $GRADLE_HOME
cd wrapper/dists
ls -ltr
```

### Useful `gradlew` Commands

- `./gradlew tasks`: Lists the tasks runnable from the root project.
- `./gradlew build`: Assembles and tests the project.
- `./gradlew clean`: Deletes the build directory.
- `./gradlew test`: Runs the unit tests.
- `./gradlew assemble`: Assembles the outputs of the project without running tests.
- `./gradlew check`: Runs all checks, including tests.
- `./gradlew dependencies`: Displays all dependencies declared in the project.
- `./gradlew help`: Displays help information about Gradle.
- `./gradlew <task> --info`: Provides additional information while running a specific task.
- `./gradlew <task> --debug`: Provides debug-level logging while running a specific task.
- `./gradlew <task> --stacktrace`: Prints out the stacktrace for any exceptions that occur during the build.

These commands can be run from the root directory of your project to perform various build and maintenance tasks.


### Java Plugin Task Graph
<br><img src="img/grdl.4.png" width=600>

