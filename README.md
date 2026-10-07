.github/workflows/build-apk.yml
name: Compilar APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Baixar projeto
        uses: actions/checkout@v4

      - name: Configurar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Configurar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extrair projeto
        run: |
          unzip -q MapaAtletasArujaV3_GitHubBuild.zip -d projeto

      - name: Compilar APK
        run: |
          cd projeto
          gradle assembleDebug

      - name: Disponibilizar APK
        uses: actions/upload-artifact@v4
        with:
          name: MapaAtletas-Aruja-APK
          path: projeto/app/build/outputs/apk/debug/app-debug.apk
