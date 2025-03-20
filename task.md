### task 1 - not working
```kotlin
import org.gradle.api.tasks.Exec

tasks.register<Exec>("generateOpenApiSpec") {
    group = "documentation"
    description = "Downloads OpenAPI v3 specification from the running Spring Boot application."

    commandLine("sh", "-c", "curl -o build/openapi/openapi.yaml http://localhost:7008/v3/api-docs.yaml")
}

// Ensure the OpenAPI spec is generated when the 'build' task is called
tasks.named("build").configure {
    dependsOn("generateOpenApiSpec")
}


/*
tasks.register<Exec>("generateOpenApiSpec") {
    group = "openapi"
    description = "Generates OpenAPI v3 specification"

    val outputFile = "$buildDir/openapi.yaml"

    commandLine = listOf(
        "java", "-jar", "build/libs/users-0.0.1-SNAPSHOT.jar",
        "--springdoc.api-docs.enabled=true",
        "--springdoc.api-docs.path=/v3/api-docs",
        "--springdoc.api-docs.swagger-ui.enabled=false",
        "--spring.output.ansi.enabled=ALWAYS"
    )

    doLast {
        println("OpenAPI spec generated at: $outputFile")
    }
}
*/


/*
tasks.register<JavaExec>("generateOpenApiSpec") {
    group = "documentation"
    description = "Generates OpenAPI v3 specification from the running Spring Boot application."

    mainClass.set("org.springframework.boot.loader.JarLauncher")
    classpath = sourceSets.main.get().runtimeClasspath
    args = listOf("--server.port=7008")

    doLast {
        val openApiJson = "http://localhost:7008/v3/api-docs"
        val outputFile = file("build/openapi/openapi.json")
        outputFile.parentFile.mkdirs()
        
        exec {
            commandLine("curl", "-o", outputFile.absolutePath, openApiJson)
        }
    }
}

// Ensure the OpenAPI spec is generated when the 'jar' task is called
tasks.named("jar").configure {
    dependsOn("generateOpenApiSpec")
}
*/
tasks.register<JavaExec>("generateOpenApiSpec") {
    group = "documentation"
    description = "Generates OpenAPI v3 specification from the running Spring Boot application."

    //mainClass.set("org.springframework.boot.loader.JarLauncher")
    mainClass.set("com.example.users.OpenApiGeneratorApplication") // Change to your actual main class

    classpath = sourceSets.main.get().runtimeClasspath
    args = listOf("--server.port=9090")

    //doLast {
        val openApiYaml = "http://localhost:9090/v3/api-docs.yaml"
        val outputFile = file("build/openapi/openapi.yaml")
        outputFile.parentFile.mkdirs()
        
        exec {
            //commandLine("curl", "-o", outputFile.absolutePath, openApiYaml)
            commandLine("touch", outputFile.absolutePath, openApiYaml)
        }
    //}
}

/*
// Ensure the OpenAPI spec is generated when the 'jar' task is called
tasks.named("build").configure {
    dependsOn("generateOpenApiSpec")
}


tasks.register<Exec>("generateOpenApiSpec") {
            group = "documentation"
            description = "Downloads OpenAPI v3 specification from the running Spring Boot application."

            commandLine("sh", "-c", "curl -o build/openapi/openapi.yaml http://localhost:9090/v3/api-docs.yaml")
}
*/

//tasks.getByName("generateOpenApiSpec").dependsOn(tasks.getByName("build"))
//tasks.getByName("build").dependsOn(tasks.getByName("generateOpenApiSpec"))
```

### let us see basic example of tasks
```kotlin
Sample:

tasks.withType<Test> {
    useJUnitPlatform()
    enabled = false
}

tasks.register<Zip>("zip-reports") {
    from("Reports/")
    include("*")
    archiveFileName.set("Reports.zip")
    destinationDirectory.set(file("/dir"))
}


tasks.named<Javadoc>("javadoc").configure {
    exclude("app/Internal*.java")
    exclude("app/internal/*")
}

tasks.register("upper") {
    doLast {
        val someString = "mY_nAmE"
        println("Original: $someString")
        println("Upper case: ${someString.toUpperCase()}")
    }
}
$ gradle -q upper
Original: mY_nAmE
Upper case: MY_NAME


tasks.register("count") {
    doLast {
        repeat(4) { print("$it ") }
    }
}

$ gradle -q count
0 1 2 3 


// springdocOpenApi {
//     outputDir.set(file("$buildDir/specs"))
//     outputFileName.set("openapi")
//     fileType.set(org.springdoc.openapi.gradle.plugin.FileType.YAML)
// }
```


The provided code snippet is a Gradle Kotlin DSL script that defines a custom task named `generateOpenApiSpec`. This task is registered using the `tasks.register` method and is configured to execute a Java application using the `JavaExec` class.

The `classpath` property is set to the runtime classpath of the main source set, which ensures that all necessary classes and dependencies are available when the task runs. This is achieved by referencing `sourceSets["main"].runtimeClasspath`.

The `mainClass.set` method specifies the main class to be executed by the task. In this case, it is set to `org.springdoc.core.OpenAPIBuilder`, which is presumably a class responsible for generating OpenAPI specifications.

The `args` method is used to pass command-line arguments to the main class. Here, two arguments are provided:
1. `--springdoc.api-docs.path=/v3/api-docs`: This argument specifies the path where the SpringDoc API documentation is available.
2. `--springdoc.output-file=build/openapi.yaml`: This argument specifies the output file where the generated OpenAPI specification will be saved, in this case, `build/openapi.yaml`.

Overall, this task automates the process of generating an OpenAPI specification for a Spring application by invoking the `OpenAPIBuilder` class with the appropriate arguments.

```kotlin
tasks.register("generateOpenApiSpec", JavaExec::class) {
     classpath = sourceSets.main.runtimeClasspath
     mainClass.set("org.springdoc.core.OpenAPIBuilder") // Use the correct class
     args("--spring.config.location=classpath:application.properties", "--springdoc.api-docs.path=/v3/api-docs", "--springdoc.output-file=build/openapi.yaml")
}
```
another approach did not work
```kotlin
//===============
springdocOpenApi {
    outputDir.set(file("$buildDir/specs"))
    outputFileName.set("openapi")
    fileType.set(org.springdoc.openapi.gradle.plugin.FileType.YAML)
}

tasks.register("generateOpenApiSpec", JavaExec::class) {
    classpath = sourceSets["main"].runtimeClasspath
    mainClass.set("org.springdoc.core.OpenAPIBuilder") // Use the correct class
    args("--springdoc.api-docs.path=/v3/api-docs", "--springdoc.output-file=build/openapi.yaml")
}
 


openApi {
    apiDocsUrl.set("http://localhost:9090/v3/api-docs.yaml")
    outputDir.set(file("specs"))
    outputFileName.set("users.yaml")
    //forkProperties.set("-Dserver.port=9090")
    waitTimeInSeconds.set(60)
} 

tasks.getByName("build").dependsOn(tasks.getByName("openApi"))
// make sure the openapi task is executed before the build task
tasks.getByName("generateOpenApiSpec").dependsOn(tasks.getByName("build"))

tasks.getByName("build").finalizedBy(tasks.getByName("generateOpenApiSpec"))
```
