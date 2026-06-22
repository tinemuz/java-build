# Soltyx Java Build

Shared Maven parent POM for Soltyx Java projects. Provides:

- **Java 25** compilation with Error Prone static analysis
- **Spotless** formatting (trim trailing whitespace, end with newline, remove unused imports)
- **Checkstyle** (Javadoc enforcement on public/protected members)
- **Surefire** test runner
- **exec-maven-plugin** (for jlink/jpackage tooling)

## Usage

Add as parent in your project's `pom.xml`:

```xml
<parent>
    <groupId>com.soltyx</groupId>
    <artifactId>java-build</artifactId>
    <version>1.0.0</version>
</parent>
```

Then your `pom.xml` only needs:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 ...">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.soltyx</groupId>
        <artifactId>java-build</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>my-project</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        ...
    </dependencies>
</project>
```

## Overriding defaults

Set properties in your project's `pom.xml` to override checkstyle config locations (must be on classpath or an absolute URL):

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <configuration>
                <configLocation>classpath:custom-checkstyle.xml</configLocation>
                <suppressionsLocation>classpath:custom-suppressions.xml</suppressionsLocation>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Publishing

```bash
mvn install                # install to local ~/.m2 for development
mvn deploy                 # deploy to configured distribution management (future)
```

Projects that depend on this must have it in their local `.m2` or in a shared repository.