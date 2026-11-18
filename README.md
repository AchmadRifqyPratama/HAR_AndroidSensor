# HAR Android Sensor - Activity Prediction

A small Android app (Java) that performs **human activity recognition (HAR)** on the phone's built-in 3-axis accelerometer using an RNN-LSTM model running on-device with TensorFlow Lite.

## How it works

- `MainActivity` registers a listener for `Sensor.TYPE_ACCELEROMETER` (`SENSOR_DELAY_NORMAL`) and shows the live X / Y / Z values.
- Samples are buffered into windows of **100 samples x 3 axes**.
- Each full window is passed to `HARClassifier`, which loads `app/src/main/ml/model_HAR_RNNLSTM_ACC.tflite` and returns probabilities for 4 classes.
- The app displays the probability for each activity: **Sitting, Jumping, Walking, Running** (rounded to 3 decimals), then starts a new window.

## Tech stack

- Java, Android Gradle Plugin 7.0, Gradle 7.0.2
- `compileSdk` 30, `targetSdk` 30, **`minSdk` 19**, ABI filter `armeabi-v7a`
- TensorFlow Lite (+ select TF ops for LSTM support), TFLite Support library, ML model binding
- AndroidX AppCompat, Material Components, ConstraintLayout

## Project structure

```
app/src/main/java/com/example/har_lstm_androidsensor/
    MainActivity.java     accelerometer listener, windowing, UI
    HARClassifier.java    TFLite interpreter wrapper
app/src/main/ml/model_HAR_RNNLSTM_ACC.tflite   trained HAR model
```

## Build and run

1. Open the project in Android Studio and let Gradle sync.
2. Run the `app` configuration on a physical Android device (ARMv7, Android 4.4+) - an accelerometer is required, so an emulator is of limited use. Or build from the command line:
   ```
   ./gradlew assembleDebug
   ```
3. Move the phone around; prediction values update after every 100 accelerometer samples.

## Notes

- The TensorFlow Lite core dependencies use `0.0.0-nightly-SNAPSHOT` versions and `settings.gradle` still lists `jcenter()`, which may need updating for a fresh build.
