# Soltyx Java Build

Shared Maven parent POM for Soltyx Java projects. Provides:

- **Java 25** compilation with Error Prone static analysis
- **Spotless** formatting (trim trailing whitespace, end with newline, remove unused imports)
- **Checkstyle** (Javadoc enforcement on public/protected members) — configs bundled automatically, no local copies needed
- **Surefire** test runner
- **exec-maven-plugin** (for jlink/jpackage tooling)

## Usage

Add as parent in your project's `pom.xml`:

```xml
<parent>
    <groupId>io.github.tinemuz</groupId>
    <artifactId>java-build</artifactId>
    <version>1.0.0</version>
</parent>
```

Checkstyle configs are auto-unpacked from `java-build-config` to `target/checkstyle-config/` at build time. No files need to be copied.

Your `pom.xml` only needs project-specific bits — plugins, dependencies, profiles:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project ...>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.soltyx</groupId>
        <artifactId>java-build</artifactId>
        <version>1.0.0</version>
    </parent>

    <artifactId>my-project</artifactId>
    <version>1.0-SNAPSHOT</version>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <executions>
                    <execution>
                        <goals><goal>check</goal></goals>
                        <phase>validate</phase>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

## Overriding checkstyle rules

Child projects can override by adding a `<configuration>` block to their `maven-checkstyle-plugin` declaration:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <configuration>
        <excludes>**/module-info.java</excludes>
    </configuration>
    <executions>
        <execution>
            <goals><goal>check</goal></goals>
            <phase>validate</phase>
        </execution>
    </executions>
</plugin>
```

For custom config locations, copy the XMLs from the `java-build` repo and set paths in `<configuration>`:

```xml
<configuration>
    <configLocation>${project.basedir}/checkstyle.xml</configLocation>
    <suppressionsLocation>${project.basedir}/checkstyle-suppressions.xml</suppressionsLocation>
</configuration>
```

## First-time setup

Add `java-build` and `java-build-config` to your local Maven repo:

```bash
git clone https://github.com/tinemuz/java-build
cd java-build
mvn install
```

## Publishing updates

```bash
mvn deploy                 # publishes to GitHub Packages
```

Requires `~/.m2/settings.xml` with a GitHub token:

```xml
<settings>
    <servers>
        <server>
            <id>github</id>
            <username>tinemuz</username>
            <password>${env.GITHUB_TOKEN}</password>
        </server>
    </servers>
</settings>
```