# Un — counting with real toys

## Product

Un teaches children aged 3–6 to count with toys on a real table. The phone or tablet lies on the table, its camera counts the toys, and a voice tutor talks the child through each lesson. The child answers by voice, so learning happens on the table, not on the screen. Everything runs on the device.

Ten adventures from counting to three up to sharing equally, in English and Russian, with a profile per child. iPhone and iPad first (TestFlight); Android, macOS, Windows and Linux to follow.

## Architecture

| Repository | What it is | Licence |
|---|---|---|
| [unengine](https://github.com/sealsbroken/unengine) | The engine and the apps: perception, speech, the lesson runtime, iOS / Android / desktop shells, the release pipeline | LGPL-3.0 |
| [unmath](https://github.com/sealsbroken/unmath) | The lessons: ten arithmetic strategies plugged into the engine | LGPL-3.0 |

```
 camera ─► toy detector (RF-DETR, ONNX Runtime / CoreML) ─► table state ─┐
                                                                         ├─► lesson (strategy) ─► tutor voice
 microphone ─► speech recognition ───────────────────────────────────────┘        ▲
                                                                          unmath lessons
```

- **Rust** core: `un-core` (state machines, strategy API), `un-vision`, `un-engine`, `un-app`; one codebase for every platform.
- **Lessons** are pure state machines behind the `Strategy` trait: events in (what was counted, what was heard), effects out (say, listen, highlight).
- **Apps**: SwiftUI (iOS), Jetpack Compose (Android) and egui (desktop) over [UniFFI](https://github.com/mozilla/uniffi-rs) bindings; platform speech on the phones.
- **Shipping**: `unengine` + `unmath` are all it takes; the Release iOS workflow tags and uploads to TestFlight.

Start with `unengine`'s README; `tools/ios.sh sim` runs the app with `unmath` cloned next to it.
