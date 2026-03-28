# 🔥 NYZEX SCRIPT v20 - COMPLETE DOCUMENTATION 🔥

```
    ███╗   ██╗██╗   ██╗███████╗███████╗██╗  ██╗
    ████╗  ██║╚██╗ ██╔╝╚══███╔╝╚══███╔╝╚██╗██╔╝
    ██╔██╗ ██║ ╚████╔╝   ███╔╝   ███╔╝  ╚███╔╝ 
    ██║╚██╗██║  ╚██╔╝   ███╔╝   ███╔╝   ██╔██╗ 
    ██║ ╚████║   ██║    ███████╗███████╗██╔╝ ██╗
    ╚═╝  ╚═══╝   ╚═╝    ╚══════╝╚══════╝╚═╝  ╚═╝
```

## 🎯 NYZEX SCRIPT - ULTIMATE ROBLOX UTILITY

**Version:** v20 | **Status:** Stable | **Platform:** PC & Mobile
---
Script: (Execute on any roblox executor)
```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/onlytestingfasterproject/NyzeXPanel/refs/heads/main/panel.lua"))()
```
---

## 🎬 INTRODUCTION

**NyzeX Script** is a premium Roblox utility script designed for maximum performance and ease of use. Whether you're playing FPS games like Arsenal, Phantom Forces, or fighting games like Blox Fruits, this script provides the competitive edge you need.

**Key Highlights:**
- ✅ **4 Advanced Aimbot Modes** with AI targeting
- ✅ **Cross-Platform Support** (PC & Mobile)
- ✅ **Smooth Performance** with minimal FPS impact
- ✅ **Customizable Everything** from colors to hitbox sizes
- ✅ **Built-in Ban System** for authorized distribution
- ✅ **Real-time GUI** with drag & minimize functionality

---

## ⌨️ KEYBOARD CONTROLS

| Key | Function | Description |
|-----|----------|-------------|
| **T** | Toggle GUI | Open/Close the main control panel |
| **R** | Aimbot ON/OFF | Enable/Disable auto-aim system |
| **C** | Cycle Target Part | Switch between Head and Body targeting |
| **V** | Cycle Aimbot Modes | Switch through 4 targeting modes |
| **Left Alt** | Hold to Zoom | Zoom in while held (PC only) |

**Mobile Controls:**
- **Long Press Screen** → Zoom in (hold), release to zoom out
- **Tap GUI Buttons** → Toggle features
- **Drag GUI** → Move window around screen

---

## ✨ COMPLETE FEATURE LIST

### 🎯 1. AIMBOT SYSTEM (4 Modes)

| Mode | How It Works | Best Used For |
|------|--------------|---------------|
| **Closest to Crosshair** | Targets player nearest to your mouse cursor | FPS games, precise aiming |
| **Closest Player** | Targets player closest to your position | Close-range combat |
| **Perfect Aim** | Prioritizes lowest health + closest distance + on-screen position | Competitive PvP |
| **AI Power** | Advanced algorithm considering velocity, health, distance, and movement patterns | Pro players, unpredictable enemies |

**Aimbot Settings:**
- **FOV Radius** (20-200): Detection area around crosshair
- **Max Distance** (50-500): Maximum range to lock onto targets
- **Target Part**: Head (instant kill) or Body (easier hits)
- **Prediction**: Calculates enemy movement for leading shots
- **Smoothness**: Controls aim speed (higher = smoother, lower = snappier)
- **Team Aimbot**: Only target enemies when Team Check is ON

### 👁️ 2. ESP (EXTRA SENSORY PERCEPTION)

**Outline ESP:**
- Highlights enemies through walls
- **Team Check Enabled:** Green = Teammate, Red = Enemy
- **Team Check Disabled:** Custom color for all players
- **8 Colors Available:** Black, Pink, Red, Blue, Yellow, Green, White

**Name Tags:**
- Displays player names above heads
- Always visible through walls
- Customizable font and size

### 📦 3. HITBOX SYSTEM

**Visual Hitbox:**
- Blue selection box around enemy's HumanoidRootPart
- Adjustable size (default: 9)
- Adjustable outline thickness (0 = invisible)
- Automatically disabled for teammates when Team Check is ON

**How It Works:**
- Expands enemy hitbox for easier targeting
- Visual outline shows exact hitbox location
- Helps understand hit registration

### 🏃 4. MOVEMENT ENHANCEMENTS

**Speed Boost:**
- Increases walk speed (default: 19)
- Adjustable from 16 to 100
- Toggle ON/OFF anytime

**FlyJump:**
- Enhanced jump power (default: 150)
- Overrides game jump physics
- Works in most games (some may require higher values)

**WallBreak (NoClip):**
- Walk through walls, floors, and obstacles
- Automatically reapplies on character respawn
- Toggle OFF to return to normal collision

**Click Teleport:**
- Click anywhere to teleport your character
- Adds 3 studs height to avoid falling through floor
- Instant positioning

### 🔍 5. VISUAL ENHANCEMENTS

**X-Ray Vision:**
- Makes all non-player parts 70% transparent
- See through walls while keeping players visible
- Toggle ON/OFF for stealth or tracking

**FOV Circle:**
- Visual circle showing aimbot detection radius
- Color: Pink with 50% transparency
- Updates in real-time with FOV changes

**Zoom System:**
- **PC:** Hold Left Alt to zoom
- **Mobile:** Long press screen to zoom
- **Zoom FOV** (10-70): Zoom level (lower = more zoom)
- **Zoom Speed** (0.1-1.0): Transition smoothness
- **Enabled by default** in v20

### 👥 6. PLAYER INTERACTION

**Follow Player:**
- Automatically walks toward target
- Teleports if distance > 50 studs
- Perfect for teaming or trolling

**Spectate:**
- Watch any player's perspective
- Camera follows target's humanoid
- Return to your character with Stop Spectate

**Teleport to Player:**
- Instant teleport to any player
- Partial name search (type "john" finds "john123")
- Adds 3 studs height for safe landing

**Instant Respawn:**
- Kill your character instantly
- Useful for resetting position or escaping glitches

---

## 🎯 AIMBOT SYSTEM DEEP DIVE

### How Each Mode Works:

**1. Closest to Crosshair**
```lua
Algorithm: 
- Calculates screen distance from crosshair to each player
- Selects player with smallest distance
- Only considers players within FOV circle
- Most precise for headshots
```

**2. Closest Player**
```lua
Algorithm:
- Calculates 3D distance from your position to each player
- Selects closest player regardless of screen position
- Best for close-range combat
- Ignores FOV circle restriction
```

**3. Perfect Aim**
```lua
Algorithm:
- Combines multiple factors:
  - Distance to player (weight: 0.5)
  - Player health percentage (weight: 2.0)
  - Screen distance from crosshair (weight: 3.0)
- Picks player with highest priority score
- Ideal for competitive play
```

**4. AI Power**
```lua
Algorithm:
- Advanced scoring system:
  + FOV circle bonus (5 points per stud closer)
  - Out-of-FOV penalty (-50 points)
  + Distance bonus (0.3 points per stud)
  + Low health bonus (2 points per 1% health)
  + Moving target bonus (+15 points if velocity > 5)
  + Head targeting bonus (+10 points)
- Selects highest AI score
- Mimics human targeting behavior
```

### Aim Smoothness Explained:
- **0.0-0.3:** Instant snap (robotic, may look suspicious)
- **0.4-0.6:** Smooth tracking (recommended for most games)
- **0.7-0.9:** Very smooth (subtle, harder to detect)
- **1.0:** Linear movement (no smoothing)

---

## 👁️ ESP & VISUAL FEATURES

### Outline ESP Properties:
| Property | Value |
|----------|-------|
| Fill Transparency | 100% (invisible) |
| Outline Transparency | 0% (fully visible) |
| Depth Mode | Always On Top (see through walls) |
| Update Rate | Real-time |

### Team Check Logic:
```
Team Check OFF → All players same color (selectable)
Team Check ON → 
  - Same team: Green (#00FF00)
  - Different team: Red (#FF0000)
```

### Name Tag Features:
- Always visible through walls
- White text with black outline (auto-generated)
- Billboard GUI follows head movement
- Automatically updates on player name change

---

## 🏃 MOVEMENT & COMBAT

### Speed Boost Mechanics:
```
Default Speed: 16 (game default)
Boost Speed: 19 (40% faster)
Max Safe Speed: 25 (some games have anti-speed)
```
**Tip:** Start with 19, increase if game allows

### FlyJump Mechanics:
```
Default JumpPower: 50
FlyJump Power: 150 (3x normal)
How it works: 
- Sets UseJumpPower = true
- Continuously reapplies (overrides game scripts)
- Works in most games
```
**If not working:** Increase power to 200-300

### WallBreak (NoClip):
```
Enables: Disables CanCollide on all character parts
Disables: Restores original CanCollide values
Persistence: Automatically reapplies on respawn
```

### Click Teleport:
```
Position: Mouse.Hit.Position + Vector3.new(0,3,0)
Why +3: Prevents falling through floor
Works on: Any clickable surface
```

---

## 👥 PLAYER INTERACTION

### Follow System:
```
Distance > 50: Teleport to target
Distance 5-50: Walk toward target
Distance < 5: Stop (within range)
Auto-updates: Every heartbeat (60 FPS)
```

### Spectate Mode:
```
Camera Subject: Target player's Humanoid
Return: Stop Spectate to return to your character
Persistence: Follows through respawns
```

### Partial Name Search:
```
Input: "john" → Finds "JohnDoe", "john123", "BigJohn"
Case-insensitive: "JOHN" = "john"
Real-time: Searches current players only
```

---

## 📱 MOBILE SUPPORT

### Touch Controls:
| Action | Function |
|--------|----------|
| **Tap GUI Button** | Toggle feature |
| **Drag GUI Title** | Move window |
| **Long Press (0.5s)** | Start zoom |
| **Release** | Stop zoom |
| **Tap Minimize** | Hide/Show GUI |

### Mobile Optimizations:
- Larger button hitboxes for touch
- Long press detection (0.5 second threshold)
- Zoom smoothness for mobile
- GUI positioned for thumb access

### Recommended Mobile Settings:
```
Aimbot Mode: Closest to Crosshair
Aim Smoothness: 0.70-0.80
FOV Radius: 80-100
Target Part: Body (easier to hit)
Team Check: ON (avoid friendly fire)
Name Tags: ON (identify players)
Zoom: ON (default)
```

---

## ⚙️ DEFAULT SETTINGS

### Core Toggles
```lua
ESP Enabled:            true        -- Outline players
Team Check:             false       -- Color by team
Hitbox Enabled:         true        -- Show blue box
Speed Boost:            false       -- Faster movement
Click Teleport:         false       -- Click to teleport
WallBreak:              false       -- Walk through walls
Name Tags:              false       -- Show names above heads
FlyJump:                false       -- Higher jumps
Aimbot:                 false       -- Auto-aim system
Show FOV Circle:        true        -- Visual detection area
X-Ray:                  false       -- See through walls
Zoom:                   true        -- Hold to zoom (NEW)
```

### Aimbot Values
```lua
Aimbot Mode:            "Closest to Crosshair"
FOV Radius:             80          -- Detection circle size
Max Target Distance:    300         -- Max lock-on range
Target Part:            "Head"      -- Head or Body
Use Prediction:         true        -- Lead moving targets
Prediction Amount:      0.17        -- How far to predict
Aim Smoothness:         0.60        -- Aim speed (0.1-1.0)
Team Aimbot Only:       false       -- Target only enemies
```

### Movement Values
```lua
Speed Amount:           19          -- Walk speed
FlyJump Power:          150         -- Jump height
```

### Hitbox Values
```lua
Hitbox Size:            9           -- Size of hitbox
Hitbox Line Thickness:  0.1         -- Outline thickness (0 = invisible)
```

### Zoom Values
```lua
Zoom FOV:               30          -- Zoom level (lower = more zoom)
Zoom Speed:             0.3         -- Transition time (seconds)
```

### Visual Values
```lua
ESP Color:              Black       -- Outline color (when Team Check OFF)
FOV Circle Color:       Pink        -- Detection circle
```

---

## 🏆 BEST SETTINGS BY GAME TYPE

### FPS Games (Arsenal, Phantom Forces, Counter Blox)
```
Aimbot Mode:            Perfect Aim
FOV Radius:             60-70
Max Distance:           250-300
Aim Smoothness:         0.55-0.65
Target Part:            Head
Prediction Amount:      0.18-0.22
Speed Amount:           19
Hitbox Size:            9
Zoom FOV:               30
ESP:                    ON (Red for enemies)
Team Check:             ON
Name Tags:              ON
```

### Fighting Games (Blox Fruits, King Legacy)
```
Aimbot Mode:            Closest Player
FOV Radius:             100-120
Max Distance:           150-200
Aim Smoothness:         0.70-0.80
Target Part:            Body
Prediction:             OFF
Speed Amount:           22-25
FlyJump Power:          200-250
Hitbox Size:            12-15
ESP:                    ON
Team Check:             ON (if teams exist)
```

### Stealth/Survival Games
```
ESP:                    ON
Team Check:             ON
Name Tags:              ON
Hitbox:                 OFF (set thickness to 0)
WallBreak:              OFF
Click Teleport:         OFF
Zoom:                   ON
Zoom FOV:               20-25 (sniper mode)
X-Ray:                  ON (optional)
```

### General PvP (All Games)
```
Aimbot Mode:            AI Power
FOV Radius:             80
Max Distance:           300
Aim Smoothness:         0.60
Target Part:            Head
Prediction:             true
Prediction Amount:      0.17
Speed Amount:           19
Hitbox Size:            9
Hitbox Thickness:       0.1
Zoom FOV:               30
ESP Color:              Pink (high visibility)
```

### Beginner Friendly
```
Aimbot Mode:            Closest to Crosshair
FOV Radius:             100
Max Distance:           200
Aim Smoothness:         0.80
Target Part:            Body
Prediction:             false
Speed Amount:           18
FlyJump:                false
Hitbox Size:            12
Hitbox Thickness:       0.2
Zoom:                   true
```

---

## 📥 INSTALLATION GUIDE

### PC (Executors)
1. Copy the entire script
2. Open your Roblox executor (Synapse X, Krnl, Scriptware, etc.)
3. Paste script into executor
4. Execute (press Inject/Execute)
5. Wait 2-3 seconds for GUI to appear
6. Press **T** if GUI doesn't show automatically

### Mobile (Executors)
1. Copy the script
2. Open mobile executor (Arceus X, Hydrogen, etc.)
3. Paste script
4. Execute
5. Long press screen for zoom
6. Tap GUI buttons to toggle features

### Manual Installation
1. Create a LocalScript in StarterPlayerScripts
2. Paste the script
3. Run the game
4. GUI will appear automatically

---

## 🔧 TROUBLESHOOTING

### GUI Not Showing
| Issue | Solution |
|-------|----------|
| Script not executed | Re-execute script |
| Game blocks GUIs | Try different game |
| GUI hidden | Press **T** key |
| Corrupted GUI | Reset character |
| Script error | Check console for errors |

### Aimbot Not Working
| Issue | Solution |
|-------|----------|
| Aimbot OFF | Press **R** to enable |
| No players | Wait for players to join |
| Out of range | Reduce Max Distance |
| Behind walls | Enable X-Ray |
| Team Check | Disable or enable Team Aimbot |

### FlyJump Not Working
| Issue | Solution |
|-------|----------|
| Game overrides physics | Increase power to 200-300 |
| Humanoid missing | Wait for character load |
| Disabled | Toggle FlyJump ON |
| Game restriction | May not work in all games |

### Zoom Not Working
| Issue | Solution |
|-------|----------|
| Zoom OFF | Toggle ON in GUI |
| Wrong key | Use Left Alt (PC) or long press (Mobile) |
| Game conflict | Some games block camera changes |
| Too smooth | Reduce Zoom Speed |

### Hitbox Not Visible
| Issue | Solution |
|-------|----------|
| Hitbox OFF | Toggle ON in GUI |
| Thickness 0 | Increase Line Thickness |
| Team Check ON | Teammates have no hitbox |
| Player no HRP | Wait for character load |

---

## ❓ FAQ

### Q: Is this script safe to use?
**A:** The script itself is safe, but using scripts in Roblox may violate some game's terms of service. Use at your own risk.

### Q: Will I get banned?
**A:** Some games have anti-cheat systems. Use responsibly and avoid obvious aimbot behavior.

### Q: Can I use this on mobile?
**A:** Yes! All features work on mobile with adapted touch controls.

### Q: Why isn't FlyJump working in my game?
**A:** Some games override jump physics. Try increasing FlyJump Power to 200-300.

### Q: How do I make hitbox invisible?
**A:** Set "Hitbox Outline Thickness" to 0 in the OTHER SETTINGS section.

### Q: Can I change the ESP color?
**A:** Yes, use the dropdown menu in the GUI under "ESP COLOR".

### Q: Does this work in all games?
**A:** Most games, but some have anti-cheat that blocks certain features.

### Q: How do I update the script?
**A:** Check GitHub for latest version and replace your script.

### Q: Can I share this script?
**A:** Yes, with proper credit to NyzeX.

### Q: What's the best aimbot mode?
**A:** AI Power for experienced players, Closest to Crosshair for beginners.

---

## 📊 PERFORMANCE METRICS

| Feature | Performance Impact |
|---------|-------------------|
| Aimbot | Low (5-10% CPU) |
| ESP | Very Low (2-5% CPU) |
| X-Ray | Medium (10-15% CPU) |
| Hitbox | Very Low (1-2% CPU) |
| Speed Boost | None |
| WallBreak | None |
| Zoom | None |
| Follow | Low (3-6% CPU) |

**Memory Usage:** 2-5 MB  
**FPS Impact:** 5-15% depending on features enabled

---

## 🎨 CUSTOMIZATION

### Adding New ESP Colors
```lua
-- In HighlightColors table:
Orange = Color3.fromRGB(255,165,0)
Purple = Color3.fromRGB(128,0,128)
Cyan = Color3.fromRGB(0,255,255)
```

### Changing Default Values
Modify the configuration section at the top of the script:
```lua
local HeadSize = 9  -- Change default hitbox size
local BoostSpeed = 19  -- Change default speed
local FlyJumpPower = 150  -- Change default jump power
local ZoomEnabled = true  -- Zoom ON by default
```
---

## 🙏 CREDITS

**Created by:** NyzeX Studio 
**Version:** 20  
**Last Updated:** March 2026  
**Special Thanks:** All testers

---

## 📜 LICENSE

This script is free to use and share.  
Credit to NyzeX Studio is appreciated but not required.

---

## 🔗 CONNECT WITH US

- **GitHub:** [Your GitHub Link]
- **Discord:** [Your Discord Server]
- **YouTube:** [Your YouTube Channel]

---

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                                                           ║
    ║     THANK YOU FOR USING NYZEX SCRIPT v20!                ║
    ║                                                           ║
    ║     "Dominate Every Game with Precision & Style"         ║
    ║                                                           ║
    ╚═══════════════════════════════════════════════════════════╝
```

**Enjoy the ultimate Roblox experience! 🚀**
