# Apo'nun Köyü — Farming, Village Life & Story-Driven Gameplay

**Grow crops. Trade your harvest. Navigate village life and local politics.**

Apo'nun Köyü is an independently developed first-person village-life game set in rural Anatolia. Built with Unity and C#, it connects farming, inventory management, trading, vehicle use, quests, dialogue, and cinematic storytelling. Economic pressure and political satire give the village its identity: earning money and dealing with local characters are part of the same experience.

The project includes both the game and the tools used to build its content: a visual quest editor, ScriptableObject definitions, dialogue and subtitle tools, world-state tracking, and save/load coordination.

**Developer:** [Alperen Baltacı](https://github.com/thetruth2003)  
**Engine:** Unity 6 · **Language:** C# · **Approach:** Modular gameplay components and data-driven content  
**Status:** In active development. The repository contains integrated gameplay systems, development scenes, and ongoing experiments.

## Gameplay at a glance

| System | What it contributes |
| --- | --- |
| Farming | Planting, watering, staged growth, crop drying, harvesting, and field-wide operations. |
| Inventory and trading | Item stacks, inventory slots, a hotbar, drag-and-drop interfaces, NPC shops, and sales. |
| Economy | Money management, product pricing, purchases, and configurable sale deductions with receipt breakdowns. |
| Quests and choices | Sequential objectives, side-quest support, action counters, location triggers, day conditions, and state-changing choices. |
| Dialogue and cinematics | NPC conversations, voiced subtitle lines, timing controls, camera transitions, and persistent cutscene sequences. |
| Vehicles | WheelCollider driving, configurable powertrain and suspension settings, entering/exiting, and farming interactions. |
| Building | Placement previews, rotation, socket snapping, and placement-validity feedback. |
| World and daily activities | Day progression, day/night presentation, and a fictional football-coupon side activity using in-game currency. |
| Persistence | Coordinated saves for quest progress, player state, inventory, crops, buildings, vehicles, and scene metadata. |
| Development tools | Quest authoring, state inspection, subtitle editing, economy configuration, and environment tools. |

The clips below are original development recordings. They show systems at different stages of production.

## Farming: from seed to harvest

<img src="Images/collect.gif" alt="Development gameplay showing crop collection and harvesting" width="800" />

Farming is built around **fields containing individual planting sockets**. Each `SeedPoint` owns a crop's runtime state, while `SeedData` supplies reusable growth configuration.

- **Planting:** assign a seed type and spawn its initial growth stage.
- **Growth:** advance through prefab stages when a new game day begins and the crop has been watered.
- **Watering:** track water state per planting socket and display a configurable visual indicator.
- **Drying:** count unwatered days and remove the crop when its drying threshold is reached.
- **Harvesting:** collect a mature crop through its `Collectable` component, then clear the planting socket.
- **Persistence:** serialize seed type, growth stage, water state, dry-day count, and treatment state.
- **Field operations:** apply planting and watering across a field through `Field.PlantAll()` and `Field.WaterAll()`.

The seed-type definitions cover crops such as wheat, corn, tomatoes, potatoes, carrots, pumpkins, peppers, beans, and melons. Crop assets determine the actual visuals and configuration.

**Source:** [SeedPoint](Assets/Scripts/farm/SeedPoint.cs) · [Field](Assets/Scripts/farm/Field.cs) · [SeedData](Assets/Scripts/player/ScritableObjects/SeedData.cs)

## Agricultural tools and vehicle interactions

<img src="Images/build.gif" alt="Development recording of field and vehicle interactions" width="800" />

Field-level operations allow machinery to interact with multiple planting sockets. The repository includes seeding, watering, and harvesting components alongside drivable vehicles.

| Component | Responsibility |
| --- | --- |
| `SeederMachine` | Detect a field through a trigger and call its planting operation. |
| `Sprinkler` | Watering interaction component. |
| `Harvester_blade` | Harvesting attachment behavior. |
| `Field` | Coordinate planting sockets and their stored state. |

The crop state also includes a pesticide-treatment flag. Further treatment effects and equipment behavior remain part of the farming design and development work.

**Source:** [Farming scripts](Assets/Scripts/farm) · [Harvesting attachment](Assets/Scripts/cars/Harvester_blade.cs)

## Trading and the village economy

<img src="Images/trade.gif" alt="Development gameplay showing the trading interface and product sales" width="800" />

Harvested items feed into the inventory and trading loop. Item definitions provide icons, prefabs, stack limits, and sale prices; the sales interface calculates a transaction from the items placed in its slots.

- NPC shop and sales interfaces connect world interactions to inventory UI.
- Sales remove the selected quantities and award money.
- Sale events update item-specific counters, allowing quests to react to what the player sells.
- `Muhasebeci` provides money management and receipt-based purchase/sale calculations.
- Configurable charges support percentage, flat, and per-kilogram calculations.
- Charge presets model commissions, transport costs, taxes, equipment costs, and household deductions.

These systems support the game's economic theme: a successful harvest is only one part of making a living. The sales UI and the newer receipt-based economy are separate integration points in the current source.

**Source:** [Sales_UI](Assets/Scripts/player/UI/Sales_UI.cs) · [Muhasebeci](Assets/Scripts/market/Muhasebeci.cs) · [ItemData](Assets/Scripts/player/ScritableObjects/ItemData.cs)

## Inventory, hotbar, and item interactions

<img src="Images/inventory.gif" alt="Development gameplay showing inventory slots and drag-and-drop interactions" width="800" />

The inventory system connects collected resources, usable items, and trading.

- Slot-based item storage with quantities and stack limits.
- Drag-and-drop slot interaction.
- Inventory and hotbar presentation.
- Item definitions carrying icons, world prefabs, usable prefabs, and seed configuration.
- Collection and sales behavior connected to gameplay state.
- Dedicated inventory persistence code.

This gives farming, interaction, and trading a shared item model instead of separate inventories for each mechanic.

**Source:** [Inventory](Assets/Scripts/player/Inventory.cs) · [Inventory UI](Assets/Scripts/player/UI) · [InventorySaveLoad](Assets/Scripts/save/InventorySaveLoad.cs)

## Modular quests and progression

The quest system describes objectives as reusable steps. `QuestEditorAsset` contains quest data, `QuestContainer` holds step definitions, and implementations of `IQuestStep` provide each objective's behavior.

```text
QuestEditorAsset
└── QuestContainer
    └── IQuestStep
        ├── BuyItemStep
        ├── SellItemStep
        ├── HarvestItemStep
        ├── TalkToNPCStep
        ├── GoToLocationStep
        ├── WaitForDayStep
        ├── ChoiceStep
        ├── SpendMoneyStep
        └── ModifyStatStep
```

- **Action objectives:** buying, selling, harvesting, and talking.
- **World objectives:** reaching an identified location or waiting for a game day.
- **Decisions and consequences:** choices and explicit changes to counters, flags, or string state.
- **Progression:** sequential step evaluation, quest chains, and side-quest handling.
- **Feedback:** UI displays the current quest, objective type, and requirements.
- **Persistence:** quest progress and supporting state can be saved and restored.

`GameStateTracker` stores named counters, flags, strings, and NPC dialogue indices. Gameplay events can update these keys, while objectives read them to determine progress.

### Location-based objectives

<img src="Images/location.gif" alt="Development gameplay demonstrating a location-triggered quest objective" width="800" />

Location identifiers and triggers connect exploration to the quest system. This allows a story step to depend on arriving at a particular place in the village.

### Quest authoring tools

A custom Unity editor supports creating and inspecting quest content. The original editor screenshots are preserved below.

<table>
<tr>
<td><img src="Images/quest1.PNG" alt="Quest development tool screenshot 1" width="400" /></td>
<td><img src="Images/quest2.PNG" alt="Quest development tool screenshot 2" width="400" /></td>
</tr>
<tr>
<td><img src="Images/quest3.PNG" alt="Quest development tool screenshot 3" width="400" /></td>
<td><img src="Images/quest4.PNG" alt="Quest development tool screenshot 4" width="400" /></td>
</tr>
</table>

**Source:** [QuestSystem](Assets/Scripts/QuestSystem) · [QuestEditorWindow](Assets/Editor/QuestEditorWindow.cs) · [GameStateTracker](Assets/Scripts/dictionary/GameStateTracker.cs)

## NPC dialogue and character-driven storytelling

<img src="Images/npc.gif" alt="Development gameplay showing an NPC interaction and dialogue" width="800" />

NPC interactions combine dialogue data, player control, camera framing, audio, and quest state.

- `NPCDialogData` organizes conversations into sections and lines.
- Lines support text, a voice clip, a delay before playback, a duration override, and a pause afterward.
- Subtitle visibility can change during delays and pauses.
- `NPCInteraction` manages the active conversation and updates its associated gameplay flag.
- Dialogue indices and named state keys provide a foundation for tracking conversations across progression.

**Relationship design:** the earlier project design also described NPC trust, character dispositions, and relationship-dependent quests, dialogue, and prices. These remain part of the intended village-life experience. The current inspected conversation code does not establish the complete 0–100 trust and pricing pipeline, so those behaviors are not presented here as fully integrated features.

**Source:** [Dialogue scripts](Assets/Scripts/diyolog) · [NPC scripts](Assets/Scripts/NPC)

## Cutscenes, cameras, and scripted movement

The cinematic layer connects story moments to quest and world triggers.

- Trigger-key-based clip selection.
- One-time, repeatable, and sequential playback modes.
- Sequence groups ordered by priority.
- Saved played-clip IDs and sequence progress.
- Subtitle timing, camera transitions, fade behavior, and movement components.
- Quest-driven activation through `CutsceneActivator`.
- Waypoint-based vehicle sequences through `CarAutoDrive`, which uses a `NavMeshAgent`.

This supports authored events such as a vehicle journey or a conversation sequence while keeping playback state across sessions.

**Source:** [CutsceneManager](Assets/Scripts/cutscene/CutsceneManager.cs) · [CarAutoDrive](Assets/Scripts/cutscene/CarAutoDrive.cs) · [CutsceneActivator](Assets/Scripts/QuestSystem/CutsceneActivator.cs)

## Vehicle driving and configuration

Vehicles have their own gameplay and tuning layer, including entering/exiting, camera support, engine audio, and a speedometer.

The newer `VehicleConfig` ScriptableObject exposes:

- Vehicle mass and drag.
- Gear ratios, differential ratio, and shift settings.
- Idle/max RPM, engine torque, and a torque curve.
- Steering angle and handbrake force.
- Suspension spring, damper, and travel.
- Forward and sideways friction stiffness.
- Front-, rear-, and all-wheel-drive configuration.

The repository also contains earlier vehicle scripts. Scene setup determines which controller and configuration are active.

**Source:** [VehicleController](Assets/Scripts/cars/yeni%20car/VehicleController.cs) · [VehicleConfig](Assets/Scripts/cars/yeni%20car/VehicleConfig.cs) · [Vehicle scripts](Assets/Scripts/cars)

## Building and world interaction

The construction component provides an interactive placement workflow:

1. Select a foundation and preview prefab.
2. Position the preview using a camera raycast.
3. Rotate it with the mouse wheel.
4. Snap to a socket when one is targeted.
5. Show green/red placement feedback.
6. Place with the left mouse button or cancel with the right mouse button.

Other world components include doors, collectable objects, tree tools, and day/night presentation.

**Source:** [BuildController](Assets/Scripts/grid/Build/BuildController.cs) · [Tree scripts](Assets/Scripts/tree) · [DayNightCycle](Assets/Scripts/sky/DayNightCycle.cs)

## Daily progression and village side activities

Game-day progression connects farming to the wider village loop. The source includes day-based quest conditions and a football-coupon side activity.

`FutbolKuponManager` models daily matches, home/draw/away selections, locked odds, combined tickets, stakes, and resolved results using in-game money. It adds another economic interaction alongside producing and selling crops.

**Source:** [GameTime](Assets/Scripts/time/GameTime.cs) · [WaitForDayStep](Assets/Scripts/QuestSystem/gorever/WaitForDayStep.cs) · [FutbolKuponManager](Assets/Scripts/market/FutbolKuponManager.cs)

## Save/load and world persistence

`SaveCoordinator` is the common entry point for manual saves, loading from the menu, and autosave triggers.

| Data | Persistence responsibility |
| --- | --- |
| Quest progress | Active step indices and quest state. |
| Player | Position, rotation, vehicle-entry state, and active vehicle identifier. |
| World | Delegated save/load operations for tools, money, buildings, vehicles, crops, and inventory. |
| Story | Dialogue/state keys and cutscene playback progress. |
| Session metadata | Day, money, last quest, scene reference, and save timestamp. |

The current implementation combines JSON files, PlayerPrefs, and existing world-save components. `SaveCoordinator` calls into `savetest` for parts of world persistence. Consolidating these paths and validating restoration across scenes and vehicle transitions remains active development work.

**Source:** [SaveCoordinator](Assets/Scripts/save/SaveCoordinator.cs) · [Save scripts](Assets/Scripts/save) · [QuestSave](Assets/Scripts/QuestSystem/QuestSave.cs)

## Architecture and development tools

The project combines ScriptableObjects for reusable content with MonoBehaviour components for scene behavior.

| Area | Main entry points |
| --- | --- |
| Item and crop data | `ItemData`, `SeedData` |
| Quest definitions and execution | `QuestEditorAsset`, `QuestContainer`, `IQuestStep`, `QuestManager` |
| Shared gameplay state | `GameStateTracker` |
| Conversations | `NPCDialogData`, `NPCInteraction` |
| Cinematic orchestration | `CutsceneManager`, `CutsceneClip` |
| Vehicle tuning | `VehicleConfig`, `VehicleController` |
| Economy | `Muhasebeci`, `ProductCatalog` |
| Persistence | `SaveCoordinator`, `QuestSave`, inventory/world save components |

The [Editor directory](Assets/Editor) contains quest authoring, a gameplay-state viewer, subtitle and cutscene editors, an economy inspector, and tree-painting tools.

## Opening the project

1. Clone this repository:

   ```bash
   git clone https://github.com/thetruth2003/Aponun_koyu.git
   ```

2. In Unity Hub, choose **Add / Add project from disk** and select the repository root.
3. Use the editor version recorded in [ProjectVersion.txt](ProjectSettings/ProjectVersion.txt): **6000.2.6f2** for the current committed project.
4. Allow Unity to resolve packages and import available assets.
5. Open the main menu or the integration test scene:

| Scene | Purpose |
| --- | --- |
| `Assets/Scenes/Main Menu.unity` | Main menu and game entry flow. |
| `Assets/Scenes/main/MAİN_TEST.unity` | Integration/sandbox scene for gameplay systems. Note the uppercase Turkish `İ` in the filename. |
| `Assets/Scenes/main/aponun orjinal koyu.unity` | Additional main village scene. |

Development scenes may contain older and newer components together. Check scene references, required assets, active objects, and startup settings when opening the project on a new machine.

## Default controls

| Input | Action |
| --- | --- |
| W A S D / arrow keys | Move |
| Mouse | Look around |
| Left Shift | Run |
| Space | Jump |
| E | Interact |
| Left mouse button | Selected tool, collection, farming, or placement action |
| Tab / F | Toggle inventory |
| Hold Q | Radial menu |
| 1–9 | Hotbar selection |
| Esc | Pause |
| V / L | Manual save / load latest save |
| Mouse wheel in build mode | Rotate placement preview |
| Right mouse button in build mode | Cancel placement |

Bindings depend on the components and Inspector settings active in the scene.

## Development direction

- Expand story and quest chains around the village's characters and locations.
- Extend the consequences of choices across dialogue and progression.
- Develop relationship-driven NPC behavior and trust-based interactions.
- Consolidate economy and persistence paths and validate scene transitions.
- Refine vehicles, agricultural tools, and the enter/exit flow.
- Expand quest history and tracking, localization, crafting, and navigation features from the original design.
- Measure scene performance and improve presentation alongside gameplay integration.

---

Developed by **Alperen Baltacı** · [GitHub](https://github.com/thetruth2003)
