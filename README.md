name: Build HamTum APK

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Set up Java JDK
        uses: actions/setup-java@v3
        with:
          distribution: 'zulu'
          java-version: '17'

      - name: Set up Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.22.2'
          channel: 'stable'

      - name: Install Dependencies
        run: flutter pub get

      # Building APK (debug mode for quick local testing without signing keys)
      - name: Build APK
        run: flutter build apk --debug

      - name: Upload APK Artifact
        uses: actions/upload-artifact@v3
        with:
          name: ham_tum_debug_apk
          path: build/app/outputs/flutter-apk/app-debug.apk
