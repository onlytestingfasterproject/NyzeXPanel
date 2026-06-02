# NyzeX Script — TeamCheck System

## Overview
TeamCheck provides colored ESP outlines, hitboxes, and aimbot teammate filtering across multiple game modes. It uses three separate detection methods that never interfere with each other.

## Detection Layers (Priority Order)

| Layer | Variable | Source | Used For |
|-------|----------|--------|----------|
| 1. Tool-Based | `toolBasedTeams` | Backpack tool scanning | Game 94117097581780 MurderMystery/FFA |
| 2. Match Data | `currentMatchData` | Remote event listeners | All other modes |
| 3. Roblox Teams | `player.Team` | Roblox engine fallback | Games with built-in teams |

---

## Color Reference

### Standard Team Colors (blueTeam/redTeam)

| Team | ESP Color | Meaning |
|------|-----------|---------|
| Your Team | No outline (skip) | Teammate — hidden |
| Blue Team | `rgb(0, 0, 255)` Blue | Enemy |
| Red Team | `rgb(255, 0, 0)` Red | Enemy |
| Unmatched | `rgb(15, 15, 60)` Dark Navy | No team found |

### Named Team Colors (custom team names)

| Condition | ESP Color | Meaning |
|-----------|-----------|---------|
| Same team name | No outline (skip) | Teammate — hidden |
| Different team name | `rgb(255, 0, 0)` Red | Enemy |
| Player not in any team | `rgb(15, 15, 60)` Dark Navy | Fallback |

### Role Colors — Game 142823291

| Role | ESP Color | Meaning |
|------|-----------|---------|
| Murderer | `rgb(255, 0, 0)` Red | Enemy |
| Sheriff | `rgb(0, 0, 255)` Blue | Ally |
| Innocent | `rgb(15, 15, 60)` Dark Navy | Neutral |
| Hero | `rgb(0, 0, 255)` Blue | Ally |

No players are skipped in this game — every role gets a color. Teammate detection uses Roblox Teams only.

### Role Colors — Game 94117097581780 (MurderMystery/FFA)

| Role | Detection Method | ESP Color |
|------|-----------------|-----------|
| Murderer | Has knife (or knife+gun) | `rgb(255, 0, 0)` Red |
| Sheriff | Has revolver/gun only | `rgb(0, 0, 255)` Blue |
| Innocent | No tools | `rgb(15, 15, 60)` Dark Navy |

No players are skipped in tool mode — every role gets a color. Teammate detection is bypassed.

### No Data Available

| Condition | ESP Color |
|-----------|-----------|
| `currentMatchData` is nil, player has Roblox Team | Player's team color |
| `currentMatchData` is nil, no Roblox Team | `rgb(15, 15, 60)` Dark Navy |
| `currentMatchData` contains `"Neutral"` | `rgb(15, 15, 60)` Dark Navy |

---

## Supported Game IDs

| Game ID | Detection Method | Modes |
|---------|-----------------|-------|
| 142823291 | Remote events (`RoundStart`, `PlayerDataChanged`) | Sheriff/Murderer/Innocent/Hero |
| 94117097581780 | Backpack tool scanning (`AddAvailableMatch`) | MurderMystery, FFA |
| 91479234935369 | Standard remote listeners | Standard team modes |
| All others | Remote listeners + Roblox Teams | blueTeam/redTeam, named teams |

---

## Remote Event Listeners

All listeners connect after a 3-second startup delay.

### General (All Games)

| Remote | Path | Data Format | Behavior |
|--------|------|-------------|----------|
| `GiveOutline` | `ReplicatedStorage.RemoteEvents` | `{ blueTeam = {...}, redTeam = {...} }` or named teams | Sets `currentMatchData`, refreshes ESP |
| `RoundUpdateFunction` | `ReplicatedStorage.Remotes.Match` | `action = "SetTeams"`, `teamsData = {...}` | Sets `currentMatchData` on `SetTeams` action |
| `StartVoting` | `ReplicatedStorage.RemoteEvents` | `{ blueTeam = {...}, redTeam = {...} }` | Sets `currentMatchData` only if both blueTeam and redTeam exist |

### Game 142823291

| Remote | Path | Data Format | Behavior |
|--------|------|-------------|----------|
| `RoundStart` | `ReplicatedStorage.Remotes.Gameplay` | `time`, `{ [playerName] = { Role = "Murderer" \| "Sheriff" \| "Innocent" \| "Hero" } }` | Builds `currentMatchData` as `{ Murderer = {...}, Sheriff = {...}, Innocent = {...}, Hero = {...} }` |
| `PlayerDataChanged` | `ReplicatedStorage.Remotes.Gameplay` | Same format as RoundStart | Updates roles mid-round |

### Game 94117097581780

| Remote | Path | Match Types | Behavior |
|--------|------|-------------|----------|
| `AddAvailableMatch` | `ReplicatedStorage.Remotes.UI` | `"MurderMystery"`, `"FFA"` | Activates tool mode only when local player is in teamData |
| `EndMatch` | `ReplicatedStorage.Remotes.Match` | — | Clears `toolBasedTeams`, deactivates tool mode |

---

## Tool Detection System

### Activation
1. `AddAvailableMatch` fires with `"MurderMystery"` or `"FFA"`
2. Local player must be in the match's teamData (prevents activating for other players' matches)
3. Tool mode activates once per match (`toolMode` guard prevents duplicates)

### Detection Logic
```
Scan each player's Character and Backpack for Tool instances:
  - Name contains "knife"     → hasKnife = true
  - Name contains "revolver" or "gun" → hasGun = true

Role assignment:
  hasKnife AND hasGun  → "Murderer"  (red)
  hasGun only          → "Sheriff"   (blue)
  hasKnife only        → "Murderer"  (red)
  neither              → "Innocent"  (dark navy)
```

### Event-Driven Watching
- `ChildAdded` / `ChildRemoved` on each player's Backpack → triggers scan
- `CharacterAdded` on each player → triggers scan after 0.5s
- `PlayerAdded` → new players are watched immediately

### Performance Optimizations
- **Debounce**: scans are rate-limited to once per 0.3 seconds
- **Change detection**: `RefreshAllESP()` and `RefreshAllHitboxes()` only run when at least one player's role changes
- **Per-player role tracking**: `knownRoles` table caches previous roles for efficient comparison
- **No periodic loops**: all scans are event-driven

---

## Teammate Detection (`IsTeammate`)

```
1. Roblox Teams check → if same Team object, return true
2. Game 142823291 → return false (no teammates, all roles visible)
3. Game 94117097581780 with toolBasedTeams → return false (all roles visible)
4. No currentMatchData OR blueTeam/redTeam → return false
5. "Neutral" team → return false
6. Named team match → return true if same team name
7. Otherwise → return false
```

---

## ESP Color Resolution (`getPlayerColor`)

```
1. Use toolBasedTeams for game 94117097581780, otherwise currentMatchData
2. If no data → player.Team color, or dark navy fallback
3. If blueTeam/redTeam → blue or red for enemies, false for teammates
4. If "Neutral" team → dark navy
5. If game 142823291 → return role color by team name
6. If game 94117097581780 with toolBasedTeams → return role color by team name
7. Same named team → false (skip ESP)
8. Different named team → red
9. Player not found → dark navy
```

---

## Default Colors

```lua
ColorRed   = Color3.fromRGB(255, 0, 0)
ColorBlue  = Color3.fromRGB(0, 0, 255)
ColorBlack = Color3.fromRGB(0, 0, 0)
```

Dark navy fallback: `Color3.fromRGB(15, 15, 60)`

---

## Setup ESP Flow

```
SetupESP(player):
  1. Skip if ESP disabled
  2. Skip if no character/humanoid/HRP
  3. Skip if Teammate (same team → no outline)
  4. Get color from getPlayerColor()
  5. If false → skip (teammate, no outline)
  6. If Color3 → use as outline color
  7. If nil → default red outline
  8. Strip game visuals (interfering Highlights/Strokes)
  9. Create Highlight with outline color
```

Hitbox and Aimbot also use `IsTeammate` and `getPlayerColor` for filtering.
