# Achievement Session State

## Overview
Lightweight World of Warcraft addon that captures the player’s currently viewed achievement category and achievement and restores that selection whenever the Blizzard Achievement UI is reopened during the same client session.

## Features
- Restores the last viewed achievement category and achievement on the next open while the client is running.
- Hooks directly into Blizzard UI events without extra dependencies.
- No user configuration or saved variables; the addon is active as soon as the Achievement UI loads.

## Slash Commands
None — the addon installs hooks automatically when the Blizzard Achievement UI becomes available.

## GUI
There is no standalone GUI. The addon hooks `AchievementFrame` show/hide scripts and `AchievementFrameCategories_SelectElementData` / `AchievementFrameAchievements_SelectElementData` to cache and reapply selections inside the Blizzard Achievement window.

## Installation
1. Copy the `AchievementSessionState` folder into your World of Warcraft `Interface/AddOns/` directory.
2. Reload the UI (`/reload`) or restart the client.

## Saved Variables
This addon deliberately avoids saved variables. State is kept in memory for the duration of the current game session.

## Implementation Notes
- Primary logic lives in [AchievementSessionState/SessionState.lua](AchievementSessionState/SessionState.lua#L1-L200).
- Hooks installed:
  - `AchievementFrame:HookScript("OnShow", ...)` restores cached state when the frame shows.
  - `AchievementFrame:HookScript("OnHide", ...)` captures the current selection before hiding.
  - `hooksecurefunc("AchievementFrameCategories_SelectElementData", ...)` and `hooksecurefunc("AchievementFrameAchievements_SelectElementData", ...)` maintain lightweight caches of the selected IDs.
- The loader waits for `Blizzard_AchievementUI` to load and also calls `InstallHooks` immediately if the UI is already active via `C_AddOns.IsAddOnLoaded`.

## Compatibility
Designed for the default Blizzard Achievement UI; behavior with third-party replacements may vary because the addon relies on specific Blizzard frame names and getters.

## License
This project is licensed under the GNU General Public License v3. See [LICENSE](LICENSE) for full terms.