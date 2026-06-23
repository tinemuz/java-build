# Java Build

Shared Maven parent POM for personal Java projects.

## What you get

- **Java 25** + Error Prone, Spotless, PMD, SpotBugs, Surefire
- Tools run automatically on `mvn validate` (PMD) and `mvn verify` (SpotBugs)
- PMD ruleset is bundled — no local config files needed

## Usage

```xml
<parent>
    <groupId>io.github.tinemuz</groupId>
    <artifactId>java-build</artifactId>
    <version>1.0.0</version>
</parent>

<artifactId>my-project</artifactId>
```

## Publishing

Push to `main` with conventional commits. Workflow auto-bumps versions, deploys to GitHub Packages, and creates a release.

| Commit | Bump |
|---|---|
| `fix:` | patch |
| `feat:` | minor |
| `feat!:` | major |

No PAT needed in CI — `GITHUB_TOKEN` handles auth. For manual deploys: `GITHUB_TOKEN=ghp_xxx mvn deploy`.