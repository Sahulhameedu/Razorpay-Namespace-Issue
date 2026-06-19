# Razorpay Namespace Issue – Temporary Fix

This repository provides a temporary workaround for the Razorpay namespace issue by replacing the broken online core package with a fixed local `.aar` file.

## Step 1: Download the Fixed AAR

Download `razorpay-core-fixed.aar` from this repository.

## Step 2: Add the AAR File to Your Flutter Project

Copy the downloaded file into:

```plaintext
android/libs/razorpay-core-fixed.aar
```

If the `libs` folder does not exist, create it manually.

Project structure:

```plaintext
your_flutter_project/
└── android/
    └── libs/
        └── razorpay-core-fixed.aar
```

---

## Step 3: Update `android/app/build.gradle`

Add the following at the bottom of `app/build.gradle`.

> Note: The syntax may vary slightly if your project uses Kotlin DSL (`build.gradle.kts`).

```gradle
dependencies {
    // Keep existing local file tree dependency
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // Force use of local fixed Razorpay AAR
    implementation files('libs/razorpay-core-fixed.aar')
}

configurations.all {
    // Prevent Gradle from downloading broken Razorpay core
    exclude group: 'com.razorpay', module: 'core'
}
```

---

## Step 4: Clean and Rebuild

Run:

```bash
flutter clean
cd android
./gradlew clean
cd ..
flutter pub get
flutter run
```

---

## Step 5: If Build Fails Due to Locked Process

Stop all Gradle background processes:

```bash
cd android
./gradlew --stop
```

Then rebuild the application.

---