# Gameplay Insights

## Insight 1: PvP combat is virtually nonexistent — the game is 99.9% PvE

**What caught my eye:**
Across 5 days and 796 matches, there are only **3 human-vs-human kills** in the entire dataset. Meanwhile, there are **2,415 bot kills**. Player-vs-player combat accounts for just **0.12%** of all kills.

**Evidence:**
- Kill (PvP): 3 events
- BotKill (PvE): 2,415 events
- BotKilled (human died to bot): 700 events
- Killed (human died to human): 3 events
- PvP kill ratio: 3 / 2,418 = **0.12%**

**Actionable recommendation:**
This signals one of two things: (a) players are actively avoiding each other and focusing on PvE looting/extraction, or (b) matchmaking puts too few humans in each session. Either way, the "battle royale" promise of human-vs-human tension is not materializing.

- **If intentional (extraction-focused design):** Lean into PvE identity. Add more bot variety, AI behaviors, and PvE objectives. Don't market as battle royale.
- **If unintentional:** Increase human density per match, add PvP incentives (bounties, contested loot zones), or shrink map sizes to force encounters.
- **Metrics affected:** PvP engagement rate, average PvP kills per match, player retention (tension drives replayability in extraction shooters).

**Why a Level Designer should care:**
If PvP is meant to be a core loop, the current maps may be too large or have too many escape routes for the human player density. Tighter chokepoints, contested loot spawns, and extraction point funnels could drive encounters. The data suggests maps are currently optimized for *avoidance*, not *engagement*.

---

## Insight 2: Ambrose Valley dominates playtime — Grand Rift is nearly abandoned

**What caught my eye:**
Map utilization is extremely lopsided:

| Map | Matches | Share |
|-----|---------|-------|
| Ambrose Valley | 566 | **71.1%** |
| Lockdown | 171 | 21.5% |
| Grand Rift | 59 | **7.4%** |

Grand Rift has roughly 1/10th the matches of Ambrose Valley.

**Evidence:**
- Ambrose Valley: 566 matches, 9,955 loot pickups, 1,799 kills
- Lockdown: 171 matches, 2,050 loot pickups, 426 kills
- Grand Rift: 59 matches, 880 loot pickups, 193 kills
- Daily trend is consistent — Grand Rift is low every single day, not a one-day anomaly

**Actionable recommendation:**
Grand Rift either has a matchmaking/rotation issue (it's rarely offered) or players are actively leaving/avoiding it. Investigate:

1. **Is it a rotation problem?** Check if the server equally offers all three maps. If Grand Rift is in rotation but low-picked, that's a design signal.
2. **Is it a quality problem?** Grand Rift may have layout issues — poor loot distribution, confusing navigation, or unappealing visuals — that make players prefer other maps.
3. **Action items:** A/B test forcing Grand Rift into rotation more frequently and measure completion rates. If completion drops vs. Ambrose Valley, it's a design issue requiring layout revision.
- **Metrics affected:** Map diversity score, session variety, content ROI (dev time spent on Grand Rift vs. engagement returned).

**Why a Level Designer should care:**
If 71% of all gameplay happens on one map, the other two maps represent wasted development effort. Level Designers should audit Grand Rift for loot balance, spawn placement, and flow — the traffic heatmap shows it has significantly less map coverage, suggesting players aren't exploring the full space.

---

## Insight 3: Southern Ambrose Valley is a kill zone — the north is a ghost town

**What caught my eye:**
Kill events on Ambrose Valley cluster heavily in the southwest and northwest quadrants, while the northeast is relatively empty.

**Evidence (quadrant kill distribution on Ambrose Valley):**

| Quadrant | Kills | Share |
|----------|-------|-------|
| Southwest (SW) | 723 | **40.2%** |
| Northwest (NW) | 626 | 34.8% |
| Southeast (SE) | 329 | 18.3% |
| Northeast (NE) | 121 | **6.7%** |

The SW quadrant has **6× more kills** than the NE quadrant. The loot heatmap confirms this pattern — the highest loot pickup density is also concentrated in the south-center (grid 0.5, 0.8 has 800 pickups, the highest of any zone).

**Actionable recommendation:**
The NE quadrant of Ambrose Valley is underutilized. This likely means:
- Loot spawns in the NE are too sparse (players have no reason to go there)
- The storm direction pushes players away from the NE early
- Spawn points cluster in the south/west

**Action items:**
1. Add high-value loot spawns to the NE to pull traffic
2. Consider adding a point of interest (objective, extraction zone) in the NE
3. Vary storm direction to sometimes push *toward* the NE instead of away
4. Review spawn point distribution — if all spawns are SW-biased, players never see 25% of the map
- **Metrics affected:** Map utilization rate (% of map area actively used), average area explored per match, loot distribution fairness.

**Why a Level Designer should care:**
A quarter of the map generating only 6.7% of combat means that area is effectively dead space. Players paid for a full map but only experience 75% of it. Redistributing loot and objectives to the NE would make the map feel larger and more varied without building new content.
