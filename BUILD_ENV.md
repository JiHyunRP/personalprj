# Build environment for Forge 1.12.2

The source targets Java 8 and ForgeGradle 2.3.

Recommended toolchain for this legacy project:

- Java/JDK 8
- Gradle 4.9–4.10.x, with 4.10.3 a practical choice for a ForgeGradle 2.3 workspace
- Minecraft Forge 1.12.2-14.23.5.2864

Example sequence after installing a compatible Gradle/JDK 8 environment:

```text
gradle setupDecompWorkspace
gradle build
```

The final jar should be under:

```text
build/libs/BiggerStacks-1.12.2-Port-0.1.0-alpha.jar
```

The included development container did not have a usable ForgeGradle workspace or network access to Maven, so the actual Minecraft build could not be executed here.
