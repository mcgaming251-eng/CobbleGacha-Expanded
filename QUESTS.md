# Quest System

CobbleGacha Expanded includes a built-in quest system with **Daily**, **Weekly**, **Monthly**, and **Event** quest categories.

This page documents how the quest system works, how rewards and rotations behave, and how server administrators can configure and reset quests.

## Quest Categories

### Daily
Daily quests are selected from the enabled Daily quest pool and rotate on the configured daily schedule.

### Weekly
Weekly quests use a separate enabled pool and rotate on the configured weekly reset schedule.

### Monthly
Monthly quests use their own pool and reset with the configured monthly rotation.

### Event
Event quests are controlled by a configured start/end window. They are intended for limited-time quests rather than the normal repeating Daily / Weekly / Monthly rotations.

## Player Progress

Quest progress is tracked per player and persists across reconnects and server restarts.

The quest board shows:

- Current progress
- Required target
- Quest description
- Reward icons
- Claim state
- Time remaining for the current quest cycle

For small goals of **10 or less**, progress notifications can appear for every increment.

For goals above **10**, HUD notifications are reduced to roughly:

- 25%
- 50%
- 75%
- 100%

The server still records every valid action; only the on-screen notification frequency is reduced.

## Supported Quest Objectives

The quest editor supports multiple objective families, including:

- Catch Pokémon
- Catch specific Pokémon species
- Catch Pokémon of a specific type
- Catch Pokémon with a specific label
- Catch shiny Pokémon
- Catch Pokémon within minimum / maximum level limits
- Match Pokémon forms
- Match Pokémon aspects
- Win Pokémon battles
- Hatch eggs
- Revive fossils
- Evolve Pokémon
- Gain Pokémon levels
- Break blocks
- Mine ores
- Chop logs
- Quarry stone
- Harvest crops
- Defeat hostile mobs

Several Pokémon conditions can be combined so a quest can require more than one property at the same time.

Example:

> Catch 3 shiny Fire-type Pokémon above level 30.

## Quest Conditions

Capture-based quests can use filters such as:

- Species
- Type
- Label
- Shiny
- Minimum level
- Maximum level
- Form
- Aspect

Conditions are evaluated server-side when progress is awarded.

## Quest Rewards

A quest can reward one or more of the following:

- **Wishstone**
- **Gacha Wishes**
- **Wishlight**
- **Wishprism**
- **Items**
- **Pokémon**

Quest cards display the configured rewards as icons. Hovering a reward shows additional information.

### Pokémon Rewards

Pokémon quest rewards use the same detailed Pokémon configuration used by the rest of CobbleGacha.

Depending on the selected reward, configuration can include:

- Species
- Form / aspect
- Level
- Shiny state
- Nature
- Ability
- Held item
- Gender
- IV settings
- Exact or randomized properties

Add-on forms can also be used when they are discovered by the Pokémon catalogue.

## Wishstone

Wishstone is the main quest-focused currency.

**160 Wishstone = 1 banked Gacha Wish.**

Wishstone is stored in the player's CobbleGacha wallet rather than as a normal inventory item.

Administrators can grant Wishstone with:

```text
/gachaquests givewishstone <player> <amount>
```

Alias:

```text
/cobblegachaquests givewishstone <player> <amount>
```

## Quest Editor

The in-game Quest Editor allows operators to manage quest definitions without manually editing files.

Administrators can:

- Create quests
- Duplicate quests
- Delete quests
- Enable / disable quests
- Edit quest IDs
- Edit titles and descriptions
- Change quest cycle
- Change progress target
- Change selection weight
- Configure objective conditions
- Configure rewards
- Configure active quest counts
- Configure reset schedules
- Configure Event date windows

Changes remain in the editor draft until they are saved.

## Quest Rotation

Each recurring category selects quests from its enabled pool.

The editor controls how many quests are active for each cycle.

Disabled quests are not selected for new rotations.

Quest weight controls how likely an enabled quest is to be selected relative to other quests in the same pool.

## Monthly Reward Budget

The Quest Editor includes a **Monthly Reward Budget** tool under the rewards/reset area.

It can rebalance the Wishstone value of recurring Daily, Weekly, and Monthly quests around a selected monthly target.

The available target range is:

**30 to 240 Wishes per reference month**

The calculation uses:

- Enabled recurring quests
- Configured active quest counts
- Daily rotations
- Weekly rotations
- Monthly quests

The budget applies to Wishstone / direct Wish rewards for recurring quests.

It does **not** overwrite:

- Event rewards
- Wishlight
- Wishprism
- Extra item rewards
- Extra Pokémon rewards

The budget is a one-time rebalance operation. If the quest pool or active counts are changed later, apply the budget again if a new balance is wanted.

## Admin Reset Controls

The Quest Editor provides two separate server-wide reset operations.

### Reset Reward Claims

Keeps player quest progress but clears claim state so completed quests can be claimed again.

### Reset All Quests + Claims

Clears quest progress and claim state across all quest durations.

Both operations also apply to offline players when their quest data is next loaded.

Already-delivered currency, items, and Pokémon are **not** removed by a reset.

The reset actions require confirmation and operator permission.

The command below is also available for administrative/testing use:

```text
/gachaquests resetall
```

Alias:

```text
/cobblegachaquests resetall
```

## Add-on Pokémon and Forms

Quest Pokémon filters and Pokémon rewards use the shared CobbleGacha Pokémon catalogue.

The catalogue supports:

- Normal Cobblemon species
- Namespaced species added by other mods
- Forms / aspects added through Cobblemon species additions
- Source filtering by the add-on that supplied the Pokémon or form

This allows add-on forms such as Starlight Fusion Pokémon to be selected and preserved as the correct form rather than falling back to the base species.

## Notes

- Quest progress and claims are server-authoritative.
- Quest rewards cannot be claimed twice for the same completed quest state.
- Large quest libraries and Pokémon catalogues are transmitted using chunked networking to avoid Minecraft's single-string packet size limit.
- Client and server should run the same CobbleGacha Expanded version.
