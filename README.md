# PavDroid V1.0.0 — Android-controlled Shock Clock 3

This is a **separate Android application** from the working Pavlok Control V3.0.3 firmware-alarm application. Do not modify the Pavlok-Control-Native-Android repository. PavDroid's Android application ID is `com.projrcc.pavdroid`, so it can install alongside Pavlok Control.

## Exact source provenance

The basis for this ZIP is the **three Pavlok Control V3.0.3 files uploaded by the user to Google Drive on November 20, 2026**, under `Pavlok/Pavlok Control`: `project.zip` (129,650 bytes), `README.md`, and `build-apk.yml.txt`. These are actual user-provided files, not files taken from an inaccessible private GitHub repository or from a previous failed PavDroid experiment.

The **original V3.0.3 `index.html` and its CSS, alarm cards, popup structure, large timer face, custom numeric MM:SS entry, presets, five-position +/- stimulus steppers, interval stepper, zap slider, tabs, and History layout** are directly retained. A separate `pavdroid-ui.js` adapts that UI for Android-owned alarms and adds only the agreed Smart Timeout, music, and snooze settings. There has been no visual rebuild.

## New architecture

- Native `PavDroidAlarmEngine` stores alarm schedules on Android and uses Android's `AlarmManager.setAlarmClock` for the next alarm. A receiver re-arms schedules on boot and clock/timezone changes.
- The Android foreground service owns alarm session state and direct BLE buzz/beep/zap output. It responds to physical middle-button tap/hold when connected.
- No PavDroid scheduled alarms or firmware timers are written to the watch. The native BLE bridge rejects firmware alarm configuration and timer writes; legacy helper files from the original source remain only to keep compatibility while this engine is verified.
- `NativeTimerEngine` from Pavlok Control is retained and modified so that Android owns countdown, Times Up, repeating stimuli, Snooze, Pause/Resume and Stop; no firmware timer starts or resets.
- Newly created alarms are enabled by default. On/off is on the Alarms page. Seven-minute snooze is the default, with editable duration and max snoozes (zero means unlimited).
- Smart Timeout is enabled by default: one hour after an alarm first fires; each Snooze moves the cutoff to two hours after the most recent Snooze. It never stops mid-snooze. Uncheck it for no session timeout.
- Include Music is optional on alarms and timers. The user selects an MP3 through Android's document picker. Playback starts 20 seconds after the first Snooze and continues until Stop, including while snoozed.
- Independent session starts are blocked while another alarm or timer is active. The engine caps direct zap commands at twelve per firing round and does not interpret queued BLE writes as proof of physical delivery.
- The blue watch/Android/lightning launcher icon is distinct from the existing Pavlok Control icon.

## Known test boundaries

**First on-device test build.** JavaScript screen tests passed, including saving an Android alarm and entering `500` as `5:00` on the original timer. Source ZIP integrity and JS syntax were checked. **The APK has not been compiled in this environment, and Bluetooth delivery, physical-button events, locked-screen alarms, automatic wakeups, and music have not been physically tested.** Keep a separate, proven wake-up alarm until those tests pass.

On Android versions requiring precise-alarm access, grant **Alarms & reminders** permission when the Alarms screen prompts for it. Android alarm saving is not the same as proving an exact wake-up was armed. PavDroid requires a connected watch for buzzes, beeps and zaps. If previously programmed firmware alarms are still stored on the Shock Clock 3, they can fire independently of PavDroid and should be disabled using the original Pavlok Control before testing both systems together.

## Three canonical GitHub paths

- `project.zip` — complete PavDroid source tree
- `README.md` — this file
- `.github/workflows/build-apk.yml` — working Gradle/SDK GitHub workflow adapted only to PavDroid labeling; it is also included inside the ZIP

Keep these filenames unchanged. When using GitHub's web UI, the workflow must be updated at the **nested** `.github/workflows/build-apk.yml` path, not uploaded as a duplicate at repository root. In Google Drive it may be stored as a separate `build-apk.yml` file alongside the ZIP and README for convenience.
