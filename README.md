# Java Build

Shared Maven parent POM for personal Java projects. Provides:

- **Java 25** compilation with Error Prone static analysis
- **Spotless** formatting (trim trailing whitespace, end with newline, remove unused imports)
- **PMD** (error-prone + performance rules, focused on actual bugs)
- **SpotBugs** (findbugs successor, High threshold)
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

PMD rules and SpotBugs config are bundled via `java-build-config` jar, auto-unpacked at build time. No local files needed.

Your `pom.xml` only needs project-specific bits:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project ...>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>io.github.tinemuz</groupId>
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

PMD runs during `validate`, SpotBugs during `verify`. Override per-project:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-pmd-plugin</artifactId>
            <configuration>
                <rulesets>
                    <ruleset>${project.basedir}/custom-ruleset.xml</ruleset>
                </rulesets>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Excluding SpotBugs false positives

Create `spotbugs-exclude.xml` in your project:

```xml
<FindBugsFilter>
    <Match>
        <Bug pattern="VA_FORMAT_STRING_USES_NEWLINE" />
        <Or>
            <Class name="com.example.MyClass" />
        </Or>
    </Match>
</FindBugsFilter>
```

Reference it:

```xml
<plugin>
    <groupId>com.github.spotbugs</groupId>
    <artifactId>spotbugs-maven-plugin</artifactId>
    <configuration>
        <excludeFilterFile>${project.basedir}/spotbugs-exclude.xml</excludeFilterFile>
    </configuration>
</plugin>
```

## First-time setup

```bash
git clone https://github.com/tinemuz/java-build
cd java-build
mvn install
```

## Publishing updates

```bash
GITHUB_TOKEN=<ghp_...> mvn deploy    # publish to GitHub Packages
```

Requires a [classic PAT](https://github.com/settings/tokens) with `write:packages` scope in `~/.m2/settings.xml`:

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