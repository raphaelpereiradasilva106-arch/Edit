
name: Gerar APK EditPro

on:
  push:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extrair projeto
        run: unzip -q editpro-mobile-fase1.zip

      - name: Testar projeto
        working-directory: editpro-mobile
        run: gradle :app:testDebugUnitTest

      - name: Compilar APK
        working-directory: editpro-mobile
        run: gradle :app:assembleDebug

      - uses: actions/upload-artifact@v4
        with:
          name: EditPro-APK
          path: editpro-mobile/app/build/outputs/apk/debug/app-debug.apk
