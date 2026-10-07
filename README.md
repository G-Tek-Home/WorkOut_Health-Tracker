# Garage Gym for Android

The Garage Gym web app wrapped in a native Android shell, plus:

- **Health Connect** sync (steps, resting heart rate, sleep, workouts). Garmin data arrives through Health Connect
  when the Garmin Connect app is set to write to it.
- **Claude** features (photo reading, recipes, meal and drink ideas) through your own Anthropic API key.
- **Full health tracking**: steps, distance, sleep with stages and a bedtime-consistency score, resting heart rate, HRV, blood oxygen,
  weight, VO2 max and workouts, with a daily readiness estimate. Daily health tips, breathwork, sauna / hot tub / cold timers,
  and energy check-ins with tips.
- **Body composition** from your smart scale: weight, BMI, body fat, muscle rate and more, with trends and goals. Share a screenshot
  from your scale app to Garage Gym and Claude reads the numbers, or let it arrive through Health Connect if your scale app supports that.
- **Blood pressure log** with averages, trend chart, safety prompts and a doctor summary, and a **blood work tracker** (scan a lab
  report photo or PDF, trends, plain-language explanation). Blood pressure readings sync from Health Connect when available.
- **Barcode scanning** (ML Kit reads the code from a photo, Open Food Facts supplies the nutrition) and **progress photos**
  stored privately on the phone.
- **Reminders** as real phone notifications (workouts, meals, water, supplements, fasting window). They come back after a reboot.
- Camera / gallery photo picker (meals, nutrition labels, recipes, rower), backup export through the Android share sheet.

All your data stays on the phone.

## Build it (about 10 minutes)

1. Install **Android Studio** (current stable): https://developer.android.com/studio
2. Unzip this folder. In Android Studio choose **File > Open** and select the `GarageGymAndroid` folder.
3. Let Gradle sync finish (first sync downloads a few hundred MB).
4. Plug in your phone with **USB debugging** on (Settings > Developer options), then press the green **Run** button.
   Or build a file to sideload: **Build > Build Bundle(s) / APK(s) > Build APK(s)**. The APK is in
   `app/build/outputs/apk/debug/app-debug.apk`. Copy it to your phone and open it
   (you will need to allow "install unknown apps" for your file manager).

If Android Studio reports a version problem, accept its suggested upgrade (Gradle, Android Gradle Plugin, or the
Health Connect library version). I could not compile this project where I wrote it, so a small fix on first build is possible.

## No computer with Android Studio? Build it online
1. Create a free GitHub account and a new private repository.
2. Upload everything inside this folder (including the hidden `.github` folder) to the repository.
3. Open the repository's **Actions** tab, choose **Build Android app**, and press **Run workflow**.
4. When it finishes (about 5 to 10 minutes), open the run and download **GarageGym-full-APK**. Unzip it and install `app-debug.apk` on your phone.
(I could not run this workflow myself, so it may need a small fix.)

## Set up inside the app

**Health Connect and Garmin**
1. Android 14+ has Health Connect built in. On Android 13 or older, install **Health Connect** from Google Play.
2. In the **Garmin Connect** app look for Health Connect under Settings > Connected Apps (wording varies by version),
   turn it on and allow it to write steps, heart rate, sleep and activities.
3. In Garage Gym open **More > Settings > Health Connect (Garmin)**, tap **Connect**, allow everything, then **Sync last 14 days**.
   After that it re-syncs once a day when you open the app.

**Claude**
1. Create an API key at https://console.anthropic.com (this is billed by usage, separate from a Claude.ai subscription).
2. In **Setup > Claude (this app)** paste the key and tap **Save**. The model name is editable.

## Samsung Health
Samsung Health cannot be read directly by outside apps (its own SDK needs Samsung's approval). Two routes work:
1. **Health Connect (live).** In Samsung Health look for Health Connect in Settings and allow it to share steps, sleep, heart rate,
   exercise, weight, body composition, water and nutrition. Then connect and sync in **More > Settings**.
   If Garmin and Samsung both write data, set the order in Health Connect > Data and access > App priority, and pick your
   preferred source in Garage Gym's settings.
2. **Export (history).** Samsung Health > Settings > Download personal data. Import the zip in **More > Settings > Samsung Health**.
   It reads steps, sleep and weight. I could not test this against a real export, so treat it as beta.

## Notes on the health data
- Allow every Health Connect permission you are comfortable with. Steps, sleep, heart rate and exercise power the core features;
  distance, weight, HRV, blood oxygen, VO2 max and breathing rate are optional extras.
- Whether sleep stages, HRV and blood oxygen show up depends on your Garmin model and what Garmin Connect shares to Health Connect.
- Readiness, sleep score and the energy insights are simple estimates for motivation, not medical measurements.

## Reminders
Open **More > Reminders**, tap **Allow notifications**, then switch on what you want. Reminders use Android's inexact alarms,
so one can arrive a few minutes late while the phone is dozing. If your phone's battery saver is aggressive, set Garage Gym
to "Unrestricted" in the app's battery settings.

## Notes

- The key is stored in the app's private storage on your phone. Anyone with root access to the phone could read it.
- Health data is read only. Nothing is uploaded except what you send to Claude (for example a meal photo).
- The app icon is the Android default. Replace `android:icon` in `AndroidManifest.xml` with your own.
