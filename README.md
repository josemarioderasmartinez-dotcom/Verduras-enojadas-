
name: Crear APK de Verduras Furiosas

on:
  workflow_dispatch:
  push:
    paths:
      - 'verduras_furiosas_android.zip'
      - '.github/workflows/android.yml'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Descargar proyecto
        uses: actions/checkout@v4

      - name: Preparar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Extraer archivos del juego
        run: unzip -o verduras_furiosas_android.zip

      - name: Preparar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Compilar APK
        working-directory: verduras_furiosas_android
        run: gradle assembleDebug

      - name: Guardar APK para descargar
        uses: actions/upload-artifact@v4
        with:
          name: Verduras-Furiosas-APK
          path: verduras_furiosas_android/app/build/outputs/apk/debug/app-debug.apk
