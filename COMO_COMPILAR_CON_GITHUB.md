# Cómo compilar BiggerStacks 1.12.2 usando GitHub (sin instalar nada en tu PC)

Este proyecto ya incluye el archivo `.github/workflows/build.yml`, que le indica a
GitHub que instale JDK 8 y Gradle 4.10.3, y compile el mod automáticamente cada vez
que subas el código. Solo tienes que subir los archivos y descargar el resultado.

## Paso 1 — Crear una cuenta de GitHub (si no tienes una)

1. Entra a https://github.com/signup y crea una cuenta gratuita.
2. Confirma tu correo electrónico.

## Paso 2 — Crear un repositorio nuevo

1. Entra a https://github.com/new
2. En "Repository name" escribe, por ejemplo: `BiggerStacksPort1122`
3. Puedes dejarlo como **Public** (recomendado: los repos públicos tienen minutos de
   GitHub Actions ilimitados/gratuitos) o **Private** (también incluye minutos
   gratuitos mensuales suficientes para esto).
4. NO marques ninguna casilla de "Add a README" ni ".gitignore" (el proyecto ya trae
   los suyos).
5. Haz clic en **Create repository**.

## Paso 3 — Subir el código del proyecto

En la página del repositorio recién creado verás un enlace que dice
**"uploading an existing file"**. Haz clic ahí (o ve a **Add file → Upload files**).

1. En tu computadora, entra a la carpeta `BiggerStacksPort1122` que descomprimiste.
2. Selecciona **todo lo que está dentro** de esa carpeta (build.gradle,
   settings.gradle, la carpeta `src`, los archivos `.md`, etc.) y arrástralo a la
   página de GitHub.
   - Importante: arrastra el **contenido** de la carpeta, no la carpeta en sí.
     El archivo `build.gradle` debe quedar en la raíz del repositorio, no dentro
     de una subcarpeta.
3. Escribe un mensaje de commit, por ejemplo "Código inicial", y pulsa
   **Commit changes**.

## Paso 4 — Añadir el flujo de compilación (workflow)

Este proyecto ya trae el archivo `.github/workflows/build.yml` incluido en el paquete
que te entregué. Si al arrastrar la carpeta tu sistema operativo ocultó la carpeta
`.github` (esto pasa a veces en Mac) y no se subió, créalo manualmente así:

1. En tu repositorio de GitHub, haz clic en **Add file → Create new file**.
2. En el campo del nombre escribe exactamente:
   `.github/workflows/build.yml`
   (GitHub creará las carpetas automáticamente al escribir las barras `/`).
3. Pega este contenido:

```yaml
name: Compilar BiggerStacks 1.12.2

on:
  push:
    branches: [ "main", "master" ]
  workflow_dispatch: {}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar el codigo del repositorio
        uses: actions/checkout@v4

      - name: Instalar JDK 8 (Temurin)
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '8'

      - name: Instalar Gradle 4.10.3
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '4.10.3'

      - name: Compilar el mod con ForgeGradle
        run: gradle build --no-daemon --stacktrace

      - name: Publicar el .jar como artefacto descargable
        uses: actions/upload-artifact@v4
        with:
          name: BiggerStacks-1.12.2-Port-jar
          path: build/libs/*.jar
          if-no-files-found: error
```

4. Pulsa **Commit changes**.

## Paso 5 — Ver la compilación en marcha

1. Ve a la pestaña **Actions** en la parte superior de tu repositorio.
2. Verás una ejecución llamada "Compilar BiggerStacks 1.12.2" con un círculo
   amarillo (en curso). Haz clic en ella para ver el progreso en vivo.
3. La primera compilación tarda más (10-20 minutos aprox.) porque ForgeGradle
   descarga Minecraft, las mappings de MCP y los parches de Forge. Es normal.
4. Cuando termine, el círculo se pondrá verde (✅ éxito) o rojo (❌ error).

## Paso 6 — Descargar el .jar compilado

1. Dentro de la ejecución ya terminada (verde), baja hasta la sección
   **Artifacts**, al final de la página.
2. Verás un archivo llamado `BiggerStacks-1.12.2-Port-jar` — haz clic para
   descargarlo. Es un .zip que contiene el .jar real dentro
   (`BiggerStacks-1.12.2-Port-0.1.0-alpha.jar`).
3. Descomprímelo y ya tienes el jar listo para poner en la carpeta `mods` de
   Forge 1.12.2.

## Volver a compilar más adelante

Cada vez que subas un cambio a los archivos del repositorio, la compilación se
disparará sola. También puedes lanzarla manualmente sin cambiar nada: en la
pestaña **Actions**, elige el workflow "Compilar BiggerStacks 1.12.2" en la lista
de la izquierda y pulsa **Run workflow**.

## Si la compilación falla (❌)

Haz clic en la ejecución fallida y luego en el paso "Compilar el mod con
ForgeGradle" para ver el error exacto. Los dos motivos más comunes en proyectos
antiguos de Forge 1.12.2 son:

- **Error de dependencias del propio plugin ForgeGradle 2.3-SNAPSHOT** (a veces
  algún artefacto viejo deja de estar disponible). Solución conocida: en
  `build.gradle`, reemplaza el bloque `buildscript` por el fork mantenido
  `anatawa12/ForgeGradle-2.3`, que corrige justamente estos problemas:

  ```groovy
  buildscript {
      repositories {
          mavenCentral()
          maven {
              name = "forge"
              url = "https://maven.minecraftforge.net"
          }
      }
      dependencies {
          classpath("com.anatawa12.forge:ForgeGradle:2.3-1.0.+") { changing = true }
      }
  }
  ```

- **Error de compilación en el código Java** (imports incorrectos, clases que no
  existen en esta versión de Forge, etc.). En ese caso pégame el log del error y
  reviso el código.
