## Click Buffer

Click Buffer remembers early released clicks in cube, ball, robot, and spider and fires them when you land or touch an orb. It can also normalize short taps in ship, UFO, and swing, plus provide three timing modes for wave.

### Features
- Early click buffering for landing/orb modes
- Minimum tap hold for ship, UFO, and swing
- Wave modes: Min Hold, Min Gap, and Grid
- Per-mode timing windows measured in physics ticks
- Two-player handling
- Pause-menu shortcut and configurable Windows hotkeys
- TPS calibration and optional debug logging

This is an input-assist mod. Use it for practice and fun; don't use it for leaderboard or submitted runs.

### Attribution
This mod's implementation is adapted from the MIT-licensed Input Buffer project by Lyophi: https://github.com/Lyoph/GMD-Buffer-Mod. See `LICENSE` for the required license notice.


Safe Jump Timing adds an experimental checkpoint-based predictor for grounded cube clicks. It tests the immediate timing and delayed candidates and chooses the earliest simulated candidate that survives the prediction horizon. If none survives or a prediction checkpoint cannot be created, it retries on later grounded frames rather than forcing an unchecked jump. This is not a guarantee of survival; see README.md for limitations and test instructions.


The pause-menu Safe Jump panel is drawn from C++ Cocos2d UI nodes and does not require a generated image or bundled UI artwork. It exposes the safe-timing toggle, window, horizon, warning, and reset controls.
