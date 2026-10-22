# Click Buffer — Geometry Dash 2.2074 (Windows)

A Geode mod source project for Geometry Dash **2.2074 on Windows**. It provides predictive safe-timing for grounded cube jumps (experimental), early-click buffering for ground/orb modes, minimum tap duration for ship/UFO/swing, and Min Hold / Min Gap / Grid timing for wave.

## Compatibility target

- Geometry Dash: `2.2074` (Windows)
- Geode SDK: `4.10.2` (matching the 2.2074 release line)
- C++ standard: C++20

Do not install this on 2.208/2.2081. The project has been prepared for the 2.2074 SDK, but **it has not been compiled or tested in this environment**. API bindings can differ between Geode releases, so the first build may reveal version-specific compile errors that need adjusting.

## Build on Windows

1. Install Geode for Geometry Dash 2.2074 and its matching SDK.
2. Install Visual Studio 2022 with the **Desktop development with C++** workload and CMake tools.
3. Set the `GEODE_SDK` environment variable to the local Geode SDK directory for 2.2074.
4. Open a Developer PowerShell for VS 2022 and run from this project folder:

   ```powershell
   cmake -B build -S .
   cmake --build build --config Release
   ```

5. The Geode build setup should package the mod as a `.geode` file. Copy it into the game's `geode\mods` folder and restart the game.

## Experimental safe cube jump timing

When enabled, a grounded cube click is intercepted. Before the next real game tick, the mod creates a temporary checkpoint and tests the immediate jump plus delayed candidate timings. It chooses the earliest candidate that does not trigger a detected death during the prediction horizon. The setting `Safe Jump Window (frames)` controls the maximum delayed candidate offset tested per planning attempt; `Prediction Horizon` controls how long each candidate is checked.

This prediction path is experimental. Checkpoint simulation can stutter, some game or third-party mod state may not be fully represented by a checkpoint, and collision/death detection may miss cases. If no candidate survives the current window—or the game does not provide a checkpoint—the mod advances a real frame and retries while the cube remains grounded. It does **not** force an unchecked/known-unsafe jump. The queued click is cancelled if the player dies/resets or is no longer grounded. Use a practice level first; if the log says checkpoint creation failed, safe timing cannot be evaluated in that situation.

## Suggested first test

1. Test in a practice level with CBF disabled and Safe Jump Timing enabled.
2. Set TPS to **240** if you have not changed the game's simulation TPS; start with Safe Jump Window = **3–5 frames** and Prediction Horizon = **90**.
3. Use a simple level with a spike pattern where an early jump and a slightly delayed jump have different outcomes.
4. Check the Geode log for messages starting with `[ClickBuffer] Safe jump selected delay` or `No survivable timing found`.
5. Then test CBF and 240 FPS. Input-handling mods can interact, so don't assume every combination behaves identically.

## Controls

Defaults: **F6** cycles main buffer window; **F7/F8** adjust TPS; **F9** toggles debug logging; **F10** cycles Wave mode. Press **Left Ctrl + Right Ctrl together** to open the replacement Click Buffer popup (the old shortcut now opens the new code-drawn menu). The shortcut hook is written for the Geode 4.10.2 / GD 2.2074 keyboard API (three-argument `dispatchKeyboardMSG` and that SDK’s `KEY_RightContol` enum spelling). The in-game pause menu also has a **Click Buffer** button that opens the same panel. The panel is a custom C++-drawn UI with a Safe Jump Timing toggle, a 1–10-frame tick slider, a 30–200-step prediction-horizon tick slider (default 90), an experimental warning, and Reset. These controls save to the same Geode settings used by the mod. Other general settings remain available in the Geode mod settings page. The slider bars use discrete clickable ticks rather than image assets.

## Attribution and license

This source is adapted from Lyophi's MIT-licensed Input Buffer project. The original MIT copyright and license notice is included in `LICENSE`. Click Buffer's mod identity/settings have been adapted for the user's project; this repository does not claim authorship of the upstream implementation.


## GitHub Actions target pinning

The project includes `.github/workflows/build.yml`, which sets `sdk: given` so the build action uses the Geode version in `mod.json` (4.10.2 for GD 2.2074). Do not set the workflow SDK to `latest`; it can build against GD 2.2081 and cause API mismatches.
