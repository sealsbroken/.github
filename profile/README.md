# Un — counting with real toys

## Product

Un teaches children aged 3–6 to count with toys on a real table. The phone or tablet lies on the table, its camera counts the toys, and a voice tutor talks the child through each lesson. The child answers by voice, so learning happens on the table, not on the screen. Everything runs on the device.

Ten adventures from counting to three up to sharing equally, in English and Russian, with a profile per child. iPhone and iPad first (TestFlight); Android, macOS, Windows and Linux to follow.

## Architecture

| Repository | What it is | Licence |
|---|---|---|
| [unengine](https://github.com/sealsbroken/unengine) | The engine and the apps: perception, speech, the lesson runtime, iOS / Android / desktop shells, the release pipeline | LGPL-3.0-or-later |
| [unmath](https://github.com/sealsbroken/unmath) | The lessons: ten arithmetic strategies plugged into the engine | LGPL-3.0-or-later |

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

## Licensing and compliance

Our engine and lesson source is published as **LGPL-3.0-or-later**. Both
[unengine](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/LICENSE)
and [unmath](https://github.com/sealsbroken/unmath/blob/f908140acde80f87e95b287af118ff1cd73101e5/LICENSE)
include the LGPL text and its incorporated GPL text, and their Cargo manifests
identify the same license. Dependencies retain their own licenses: the LGPL
label does not replace third-party terms.

The review below explains the licensing basis, the evidence checked, and the
conditions for compliant distribution. **It does not certify that every app
archive or model download already satisfies every license.** The desktop GPL
speech dependency and unresolved voice-model terms are material distinctions.

### What was checked

On **18 October 2026**, we reviewed `unengine` at
[`3a540cb`](https://github.com/sealsbroken/unengine/tree/3a540cb38ee245bbe79bbaed64483159ba1f893a)
and `unmath` at
[`f908140`](https://github.com/sealsbroken/unmath/tree/f908140acde80f87e95b287af118ff1cd73101e5).
The [dependency inventory](https://github.com/sealsbroken/.github/blob/main/profile/license-inventory.json)
records package versions, declared license expressions, repository commits, and
lockfile hashes from four separate `cargo metadata --locked` resolutions:

| Build entry point | Resolved Rust packages | Missing license metadata |
| --- | ---: | ---: |
| Engine workspace | 543 | 0 |
| Lesson module | 27 | 0 |
| Mobile library | 147 | 0 |
| Desktop product | 536 | 0 |

The sets overlap: together they contain **561 distinct package/version/source
entries**, not the sum of the rows. They include optional, development, and
cross-platform dependencies; they are not lists of everything inside a
particular executable. A declared license is evidence to review, not proof
that all files, notices, or distribution conditions have been checked.

### Why the source-license choices can work together

The LGPL permits reuse of the covered libraries while preserving recipients'
rights to inspect, modify, and redistribute them. Publishing the engine and
lessons with their license texts, manifests, and build sources makes those
terms explicit and gives recipients the source for those versions. Third-party
source and bundled materials still need their own notices and permissions.

Dual licensing offers a choice; combined licensing imposes multiple conditions.
For example, `MIT OR Apache-2.0` allows a permitted alternative, while
`(MIT OR Apache-2.0) AND Unicode-3.0` also requires the Unicode terms.
`epaint_default_fonts` similarly includes separate font obligations. We cannot
approve a dependency merely because one familiar license name appears in its
metadata.

| License group found in the Rust resolutions | How it fits, and what must be preserved |
| --- | --- |
| MIT, Apache-2.0 and their alternatives | These allow reuse subject to their conditions. Preserve applicable copyright and license notices; Apache distribution also requires its license, relevant NOTICE content, and modification notices where applicable. Neither is a right to erase upstream attribution. |
| BSD-2-Clause, BSD-3-Clause, ISC, Zlib, IJG and BSL-1.0 | These have their own attribution, notice, and modification conditions. Keep the actual component license text rather than treating every permissive license as interchangeable. `mozjpeg-sys` declares several requirements together. |
| 0BSD, MIT-0, CC0-1.0 and Unlicense alternatives | Select an available licensing route and record it. These alternatives do not remove another component's obligations. |
| MPL-2.0, including UniFFI and option-ext | MPL has file-level copyleft. Distribution must provide access to covered source and preserve notices; modified covered files remain subject to MPL. A combination using MPL's secondary-license provisions must also satisfy their conditions. |
| Unicode-3.0 | Preserve the applicable Unicode copyright and permission notice, including when Unicode data accompanies a dependency. |
| OFL-1.1 and Ubuntu-font-1.0 | Embedded fonts retain their font licenses. Preserve notices and observe the applicable naming and redistribution restrictions when modifying or redistributing fonts. The Rust wrapper's MIT/Apache option does not replace these terms. |
| CDLA-Permissive-2.0, in webpki certificate-root data | The data has separate terms, including carrying the agreement when sharing the data. It is not relicensed merely by compiling the Rust wrapper. |

These are a compatibility and obligation map, not an assertion that packaging
already includes every required notice. The governing sources include the
[Apache license](https://www.apache.org/licenses/LICENSE-2.0),
[MPL guidance](https://www.mozilla.org/en-US/MPL/2.0/FAQ/),
[Open Font License](https://openfontlicense.org/open-font-license-official-text/),
and [CDLA terms](https://cdla.dev/permissive-2-0/), alongside each package's own
license files.

### Mobile and desktop have different distribution obligations

For a combined application using our LGPL libraries,
[LGPL section 4](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/LICENSE)
requires notices and license copies, and a qualifying mechanism for using a
modified library. Static linking requires particular attention to corresponding
source/application code suitable for recombination or relinking. Installation
information must also be supplied when applicable. A public source repository
alone does not demonstrate that a signed, distributed app meets all of these
conditions. App-store delivery is not itself a compliance exemption.

The [mobile manifest](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/apps/mobile/Cargo.toml)
uses the platform-speech FFI path; that path does not enable the desktop
`un-speech` dependency. This avoids that particular desktop speech component,
while leaving LGPL, native-runtime, platform-SDK, and other distribution
requirements to be checked for the actual mobile build.

The [engine's license notes](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/README.md#licence)
identify **espeak-ng, GPL-3.0**, in the prebuilt desktop speech stack. The
[Windows release workflow](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/.github/workflows/release-windows.yml)
therefore describes its combined bundle as GPL-3.0. Such a distribution needs
the applicable GPL terms, notices, and complete corresponding source, including
relevant third-party components and build material. Declaring our own source
LGPL does not make that bundle permissive or closed-source. The
[upstream espeak-ng license](https://github.com/espeak-ng/espeak-ng/blob/master/COPYING)
is separate from sherpa-onnx's Apache license and the Rust wrapper's MIT license.
Native-library contents must be inspected, not inferred from wrapper metadata.

### Model weights, voices, and training data are separate works

The [detector training script](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/tools/train_detector.py)
selects RF-DETR Nano, Small, or Medium. Roboflow identifies Apache-designated
RF-DETR code and weights under its
[licensing policy](https://roboflow.com/licensing). The annotation pipeline uses
OWLv2 through Transformers. A release still needs the exact base-model version,
weight provenance, applicable notices, and evidence of rights to the training
images and annotations. Training a model ourselves does not establish all of
those rights. The engine documents Open Images attribution requirements; those
records must be matched to the dataset actually used.

Optional Ultralytics-based tooling remains in the source tree. Its
[upstream licensing](https://www.ultralytics.com/license)
requires a separate assessment when that route is used. Exporting a model to
ONNX does not by itself remove its license conditions. A tool's presence is also
not evidence that a particular distributed detector was produced with it.

Speech models need their own review. The configured Russian **Irina** Piper
model card reports its dataset license as **Unknown**; the English **Lessac**
card points to separate dataset terms. Those cards were inspected in the local
model downloads. The [fetch script](https://github.com/sealsbroken/unengine/blob/3a540cb38ee245bbe79bbaed64483159ba1f893a/tools/fetch_models.py)
selects both voices, and the Windows workflow copies both into its package.
A general Apache/MIT description of speech software does not resolve these
model-specific rights. Irina's redistribution clearance remains unresolved in
this review; Lessac's exact terms also need verification for the shipped asset.

### What remains to establish complete release compliance

Before calling a particular release compliant, its review must account for the
actual files delivered: source commits and build instructions, native libraries,
license and copyright notices, MPL source access, font licenses, model cards,
dataset provenance, and applicable LGPL/GPL relinking or source requirements.
The Cargo inventory does not include Python or Gradle transitive dependencies,
native binary contents, platform agreements, or a file-by-file provenance audit.

In particular, the inspected Windows packaging steps copy executable libraries
and models but do not explicitly assemble a complete license/notice bundle or
complete corresponding-source package. Some model archives may carry notices,
but that is not evidence of complete coverage. This review did not unpack and
certify every published release or signed mobile app.

The verified result is an explicit source license and a reproducible dependency
inventory with documented distribution obligations. Full compliance of every
shipped artifact cannot yet be claimed from that evidence alone. Report missing
attribution or licensing evidence to [lead@onym.app](mailto:lead@onym.app).

## Community

Read our [contribution guidelines](https://github.com/sealsbroken/.github/blob/main/CONTRIBUTING.md)
and [Code of Conduct](https://github.com/sealsbroken/.github/blob/main/CODE_OF_CONDUCT.md).
Contact: [lead@onym.app](mailto:lead@onym.app).
