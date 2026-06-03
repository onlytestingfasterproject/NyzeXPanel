# NyzeX Script V37 (Extra ⚡)

```
    ███╗   ██╗██╗   ██╗███████╗███████╗██╗  ██╗
    ████╗  ██║╚██╗ ██╔╝╚══███╔╝██╔════╝╚██╗██╔╝
    ██╔██╗ ██║ ╚████╔╝   ███╔╝ █████╗   ╚███╔╝ 
    ██║╚██╗██║  ╚██╔╝   ███╔╝  ██╔══╝   ██╔██╗ 
    ██║ ╚████║   ██║    ███████╗███████╗██╔╝ ██╗
    ╚═╝  ╚═══╝   ╚═╝    ╚══════╝╚══════╝╚═╝  ╚═╝
```

## Overview

Multi-feature Roblox script with ESP, Aimbot, Hitbox, Trigger Bot, NPC Support, and more. Includes game-specific auto-enable dialogs for TeamCheck and NPC Support.

## 📥 QUICK INSTALLATION

**Execute on any Roblox executor:**
```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/onlytestingfasterproject/NyzeXPanel/refs/heads/main/panel.lua"))()
```

---

## Features

| Feature | Default | Description |
|---------|---------|-------------|
| **ESP** | ON | Colored player outlines (enemy highlight) |
| **Hitbox** | ON | Expanded hitbox parts for easier targeting |
| **Zoom** | ON | Hold Left Alt to zoom (FOV-based) |
| **Show FOV Circle** | ON | Display aimbot FOV circle |
| **Aimbot Prediction** | ON | Leading target movement prediction |
| **Speed** | OFF | Walk speed boost |
| **Click TP** | OFF | Teleport to mouse click position |
| **WallBreak** | OFF | Noclip through walls |
| **Team Check** | OFF | Skip teammates in ESP, Hitbox, Aimbot |
| **Name Tag** | OFF | Show player names above heads |
| **FlyJump** | OFF | Enhanced jump/fly movement |
| **Xray** | OFF | See players through walls |
| **Trigger Bot** | OFF | Auto-fire when crosshair on enemy |
| **NPC Support** | OFF | Enable NPC targeting for all systems |
| **Mouse Unlock** | — | Press P to unlock mouse from FPS camera |

---

## TeamCheck System

### Detection Layers (Priority Order)

| Layer | Variable | Source | Used For |
|-------|----------|--------|----------|
| 1. Tool-Based | `toolBasedTeams` | Backpack tool scanning | Game 94117097581780 (MurderMystery/FFA) |
| 2. Match Data | `currentMatchData` | Remote event listeners | All other modes |
| 3. Roblox Teams | `player.Team` | Roblox engine fallback | Games with built-in teams |

### Color Reference

#### Standard Team Colors (blueTeam/redTeam)

| Team | ESP Color | Meaning |
|------|-----------|---------|
| Your Team | No outline (skip) | Teammate — hidden |
| Blue Team | `rgb(0, 0, 255)` Blue | Enemy |
| Red Team | `rgb(255, 0, 0)` Red | Enemy |
| Unmatched | `rgb(15, 15, 60)` Dark Navy | No team found |

#### Named Team Colors

| Condition | ESP Color | Meaning |
|-----------|-----------|---------|
| Same team name | No outline (skip) | Teammate — hidden |
| Different team name | `rgb(255, 0, 0)` Red | Enemy |
| Player not in any team | `rgb(15, 15, 60)` Dark Navy | Fallback |

#### Role Colors — Game 142823291

| Role | ESP Color | Meaning |
|------|-----------|---------|
| Murderer | `rgb(255, 0, 0)` Red | Enemy |
| Sheriff | `rgb(0, 0, 255)` Blue | Ally |
| Innocent | `rgb(15, 15, 60)` Dark Navy | Neutral |
| Hero | `rgb(0, 0, 255)` Blue | Ally |

#### Role Colors — Game 94117097581780 (MurderMystery/FFA)

| Role | Detection Method | ESP Color |
|------|-----------------|-----------|
| Murderer | Has knife (or knife+gun) | `rgb(255, 0, 0)` Red |
| Sheriff | Has revolver/gun only | `rgb(0, 0, 255)` Blue |
| Innocent | No tools | `rgb(15, 15, 60)` Dark Navy |

#### Game 130692467183507

| Condition | ESP Color |
|-----------|-----------|
| All players | `rgb(255, 0, 0)` Red — everyone is enemy |

#### Game 86178279217218 — Container + Highlight detection

| Condition | ESP Color | System |
|-----------|-----------|--------|
| Teammate (has game Highlight) | Roblox TeamColor (visible) | ESP — team color outline |
| Enemy (no game Highlight) | `rgb(255, 0, 0)` Red | ESP, Hitbox, Aimbot |
| Fallback (no match data) | `rgb(15, 15, 60)` Dark Navy | ESP only — Hitbox/Aimbot disabled |

#### No Data Available

| Condition | ESP Color |
|-----------|-----------|
| `currentMatchData` is nil, player has Roblox Team | Player's team color |
| `currentMatchData` is nil, no Roblox Team | `rgb(15, 15, 60)` Dark Navy |
| `currentMatchData` contains `"Neutral"` | `rgb(15, 15, 60)` Dark Navy |

### Teammate Detection (`IsTeammate`)

```
1. Roblox Teams check → if same Team object, return true
2. Game 142823291 → return false (no teammates, all roles visible)
3. Game 94117097581780 with toolBasedTeams → return false (all roles visible)
4. Game 130692467183507 → return false (no teammates, everyone enemy)
4. Game 130692467183507 → return false (no teammates, everyone enemy)
5. No currentMatchData OR blueTeam/redTeam → return false
6. "Neutral" team → return false
7. Named team match → return true if same team name
8. Otherwise → return false
```

### ESP Color Resolution (`getPlayerColor`)

```
1. Game 130692467183507 → return red (all players enemy)
2. Use toolBasedTeams for game 94117097581780, otherwise currentMatchData
3. If no data → player.Team color, or dark navy fallback
4. If blueTeam/redTeam → blue or red for enemies, false for teammates
5. If "Neutral" team → dark navy
6. If game 142823291 → return role color by team name
7. If game 94117097581780 with toolBasedTeams → return role color by team name
8. Same named team → false (skip ESP for all games except 86178279217218)
9. Different named team → red
10. Player not found → dark navy
```

Note: When `getPlayerColor` returns `false` (teammate), ESP is skipped for all standard games. For game `86178279217218`, `false` is overridden to show the player's Roblox TeamColor. Hitbox and Aimbot always skip `false`-returned players regardless of game.

### Remote Event Listeners (TeamCheck)

All listeners connect after a 3-second startup delay.

#### General (All Games)

| Remote | Path | Data Format | Behavior |
|--------|------|-------------|----------|
| `GiveOutline` | `ReplicatedStorage.RemoteEvents` | `{ blueTeam = {...}, redTeam = {...} }` or named teams | Sets `currentMatchData`, refreshes ESP |
| `RoundUpdateFunction` | `ReplicatedStorage.Remotes.Match` | `action = "SetTeams"`, `teamsData = {...}` | Sets `currentMatchData` on `SetTeams` action |
| `StartVoting` | `ReplicatedStorage.RemoteEvents` | `{ blueTeam = {...}, redTeam = {...} }` | Sets `currentMatchData` only if both blueTeam and redTeam exist |

#### Game 142823291

| Remote | Path | Data Format | Behavior |
|--------|------|-------------|----------|
| `RoundStart` | `ReplicatedStorage.Remotes.Gameplay` | `time`, `{ [playerName] = { Role = "Murderer" \| "Sheriff" \| "Innocent" \| "Hero" } }` | Builds `currentMatchData` as role-grouped table |
| `PlayerDataChanged` | `ReplicatedStorage.Remotes.Gameplay` | Same format as RoundStart | Updates roles mid-round |

#### Game 94117097581780

| Remote | Path | Match Types | Behavior |
|--------|------|-------------|----------|
| `AddAvailableMatch` | `ReplicatedStorage.Remotes.UI` | `"MurderMystery"`, `"FFA"` | Activates tool mode only when local player is in teamData |
| `EndMatch` | `ReplicatedStorage.Remotes.Match` | — | Clears `toolBasedTeams`, deactivates tool mode |

#### Game 86178279217218 — Container + Highlight detection

| Mechanism | Path / Source | Behavior |
|-----------|---------------|----------|
| Player list | `PlayerGui.TopGui.GameStats.Container` — Frame children (name = player) | Scans Frame children for player names |
| Team detection | Character `Highlight` instances (except NyzeX_ESP) | Has Highlight = teammate; no Highlight = enemy |
| 1v1 shortcut | Container has exactly 2 frames | Other player = enemy (no Highlight scan) |
| EndGame | `ReplicatedStorage.Events.Round.OnClientEvent` → `"EndGame"` | Clears `currentMatchData`, removes all ESP/Hitbox |
| New match | Container child changes | Resets `matchEnded` flag, re-runs team scan |
| StripGameVisuals | Exception for 86178279217218 | Game's native Highlight instances are NOT destroyed |

### Tool Detection System (Game 94117097581780)

- Activated by `AddAvailableMatch` with `"MurderMystery"` or `"FFA"`
- Local player must be in the match's teamData
- Scans each player's Character and Backpack for Tools
- Name contains `"knife"` → Murderer (red)
- Name contains `"revolver"` or `"gun"` → Sheriff (blue)
- Neither → Innocent (dark navy)
- Debounced (0.3s) and event-driven (ChildAdded/Removed, CharacterAdded, PlayerAdded)

### Game-Specific Edge Cases

#### StripGameVisuals Exception

| Game | Behavior |
|------|----------|
| All games except 86178279217218 | Game `Highlight`/`SelectionBox`/`UIStroke` instances are destroyed |
| 86178279217218 | Game `Highlight` instances are preserved (required for team detection) |

#### Fallback State (no currentMatchData)

| Game | ESP Color | Hitbox | Aimbot |
|------|-----------|--------|--------|
| 86178279217218 | Dark navy (`15,15,60`) or Roblox TeamColor | Disabled | Disabled |
| All others | Roblox TeamColor or dark navy | Normal behavior | Normal behavior |

When game `86178279217218`'s match ends (`EndGame` remote event), `currentMatchData` is set to `nil`, all ESP/Hitbox are cleaned up, and `matchEnded` flag prevents re-scans. A new match is detected when new Frame children appear in the Container.

---

## NPC Support System

### How It Works

1. `StartNPCScan()` runs `ScanAndTrackNPCs()` on Heartbeat (throttled to 2s intervals)
2. Scans workspace descendants for valid NPC models (Model + Humanoid + HumanoidRootPart, not a Player)
3. Tracks NPCs in `NPCTracker` table with visual instances

### NPC Detection Criteria

- Must be a `Model`
- Must not be the local player's character
- Must not belong to a Player
- Must have a Humanoid with Health > 0
- Must have a HumanoidRootPart

### NPC Visuals Applied

| Visual | Description |
|--------|-------------|
| **ESP** | Yellow `Highlight` outline (when ESP is enabled) |
| **NameTag** | `BillboardGui` showing NPC name (when NameTag is enabled) |
| **Hitbox** | `SelectionBox` with expanded HumanoidRootPart (when Hitbox is enabled) |

### NPC Aimbot Integration

When NPC Support is enabled, all 4 aimbot modes include NPC targets alongside players:
- **Closest to Crosshair** — includes NPCs in screen-distance calculation
- **Closest Player** — includes NPCs in distance calculation
- **Perfect Aim** — includes NPCs in priority scoring
- **AI Power** — includes NPCs in AI scoring

### NPC Trigger Bot Integration

When NPC Support is enabled, Trigger Bot fires at NPC models tracked in NPCTracker.

---

## Aimbot System

### Modes

| Mode | Targeting Strategy |
|------|--------------------|
| **Closest to Crosshair** | Picks target with smallest screen distance from crosshair |
| **Closest Player** | Picks target with smallest 3D world distance |
| **Perfect Aim** | Scores targets by distance, health, and angle priority |
| **AI Power** | Uses a composite AI score with target switching delay |

### Aimbot Settings

| Setting | Control | Default |
|---------|---------|---------|
| FOV Radius | Slider | 200 |
| Target Part | Toggle (Head/Body) | Head |
| Show FOV Circle | Toggle | On |
| Prediction Amount | Slider | 0.1 |
| Aim Smoothness | Slider | 0.8 |
| Max Distance | Slider | 1000 |
| Target Stickiness | Slider | 0.3s |
| AI Switch Delay | Slider | 1.5s |
| AI Switch Threshold | Slider | 15% |
| Use Prediction | Toggle | On |

### Lock Target (L key)

- Locks aimbot onto current target
- Sticky target persists briefly (stickiness duration) after target moves off crosshair
- Locked target is validated each frame (must be valid player/NPC, within FOV, not teammate)

---

## Trigger Bot

- Scans a grid around the crosshair using raycasts
- Fires when crosshair is on an enemy player or NPC (with NPC Support)
- Configurable cooldown (default 0.1s)
- Configurable scan radius (default 4)
- Runs on RenderStepped
- Uses `VirtualInputManager:SendMouseButtonEvent()` for input simulation

---

## Game Support Dialogs

### Supported Game IDs

#### TeamCheck Games

| Game ID | Detection Method | Modes |
|---------|-----------------|-------|
| 142823291 | Remote events (`RoundStart`, `PlayerDataChanged`) | Sheriff/Murderer/Innocent/Hero |
| 94117097581780 | Backpack tool scanning (`AddAvailableMatch`) | MurderMystery, FFA |
| 91479234935369 | Standard remote listeners | Standard team modes |
| 18974202390 | Standard remote listeners | Standard team modes |
| 155615604 | Standard remote listeners | Standard team modes |
| 4580204640 | Standard remote listeners | Standard team modes |
| 3214114884 | Standard remote listeners | Standard team modes |
| 130692467183507 | All players shown as red (no detection) | FFA / Everyone enemy |
| 11276071411 | Standard remote listeners | Standard team modes |
| 126922689754590 | KillerFolder/PlayersFolder detection (killer=red, players=blue) | Detective-style game |
| 86178279217218 | Container frames + game Highlight (has HL = teammate, no HL = enemy); 1v1 shortcut | Team vs enemy — teammates shown with Roblox TeamColor in ESP |

#### NPC Support Games

| Game ID | Description |
|---------|-------------|
| 116495829188952 | Game with NPC enemies |
| 115286378269814 | Game with NPC enemies |
| 16389395869 | Game with NPC enemies |

### Auto-Enable Countdown

When the script detects a supported game, a dialog appears after 4 seconds:

```
Dialog shows → Countdown starts at 20s
  Every 1s: "Auto-enabling in Xs"
  Click "Enable" → enables immediately
  Timer hits 0 → enables automatically
  Fade-out animation (0.3s) → GUI destroyed
```

- **TeamCheck dialog**: Explains TeamCheck features, enables `TeamCheckEnabled`
- **NPC Support dialog**: Explains NPC Support features, enables `NpcSupportEnabled`

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **T** | Toggle GUI panel (minimize/maximize) |
| **R** | Toggle Aimbot ON/OFF |
| **C** | Cycle target part (Head ↔ Body) |
| **V** | Cycle aimbot mode |
| **L** | Lock / unlock current target |
| **P** | Unlock mouse from FPS camera lock |
| **Left Alt** | Hold to zoom |

---

## GUI Controls

### Main Toggle Buttons

- Speed, ESP, Hitbox, Click TP, WallBreak, Team Check, Name Tag, FlyJump, Aimbot, Xray, Zoom, TriggerBot, NPC Support

### Mode/Action Buttons

- Change Mode (cycles through 4 aimbot modes)
- Head Target / Body Target
- Respawn Character
- Start/Stop Follow
- Spectate / Stop Spectate
- Teleport to Player

### Sliders

- FOV Radius, Speed Amount, Hitbox Size, Hitbox Outline Thickness, FlyJump Power, Zoom FOV, Zoom Speed
- Target Stickiness, AI Switch Delay, AI Switch Threshold, Max Distance
- Prediction Amount, Aim Smoothness

### Save/Load Settings

| Button | Action |
|--------|--------|
| Save Settings | Saves all toggles/sliders to `NyzeX_Settings_<PlaceId>.json` |
| Load Settings | Loads saved settings and applies them |
| Reset to Defaults | Deletes saved file, resets everything to defaults |

### Auto-Load on Startup

If saved settings exist for the current game, a confirmation dialog appears after 2 seconds asking to load them.

---

## Mouse Unlock (P Key)

- Toggles `MouseUnlockOverride` to force mouse unlock from FPS camera lock
- Uses `RenderStepped` connection to continuously override game's mouse lock
- Shows notification: "Mouse locked by game — press P to unlock" when game locks mouse
- Forces cursor icon visibility

---

## Click Teleport

- When enabled, left-clicking on a surface teleports the local player to that position
- Uses `workspace:Raycast()` from mouse position

---

## WallBreak (Noclip)

- Enables/disables noclip on the local player's character
- Works on all character parts via `Stepped` connection
- Toggle on/off dynamically

---

## FlyJump

- Enhanced jump with configurable power
- Increases `JumpPower` / `JumpHeight` on the Humanoid

---

## Xray

- Makes all non-character parts transparent to see players through walls
- Toggle on/off

---

## Settings Persistence

Settings are saved per-game as JSON files:

- **File format**: `NyzeX_Settings_<PlaceId>.json`
- **Contents**: All toggle states, slider values, target part, aimbot mode
- **Location**: Executor's writable directory (`writefile`)
- **Auto-load**: Optional prompt on script start

---

## Default Colors

```lua
ColorRed   = Color3.fromRGB(255, 0, 0)
ColorBlue  = Color3.fromRGB(0, 0, 255)
ColorBlack = Color3.fromRGB(0, 0, 0)
```

Dark navy fallback: `Color3.fromRGB(15, 15, 60)`
