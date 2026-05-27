# Prestige & Draft Mode
**Wrath of the Lich King (3.3.5a)**

![image](https://github.com/user-attachments/assets/5d3ed4b4-0765-4238-902e-cf2ad82fd8d0)


A minimally invasive prestige system that lets characters reset back to level one in exchange for unique rewards and progression options. It features both a classic prestige reset and an optional Draft Mode, where players select a new spell from a randomized pool upon each level-up. 

---

## ✨ Key Features

* **Classic Prestige Reset:** Return to level 1 while retaining unique progression titles.
* **Draft Mode:** At each level‑up, choose from a randomized pool of three spells.
* **Spell Rarity System:** Player‑usable spells are bucketed into five rarity tiers (Common, Uncommon, Rare, Epic, Legendary), with configurable distribution to ensure balanced and meaningful draft pools.
* **AutoLearn:** Spells you've drafted automatically scale with your level. Each time you draft a new spell, all previously learned spells are upgraded to their highest rank available for your current level.
* **Ban System:** Players are given a configurable number of bans at the start of each draft run, allowing them to strategically exclude unwanted spells from appearing in their draft pool.
* **Minimalist Classic+ UI:** A clean, unobtrusive interface inspired by Classic Wow design. Spell details appear only on hover, preserving immersion while offering modern draft functionality.
* **Visual Feedback:** Smooth fade-out animations and subtle transitions enhance the drafting experience, providing satisfying, polished feedback when selecting spells. Clean visuals make every choice feel impactful.
---

**Client Addon/Patch are NOT required to enjoy your server.**

**Clientside users have to make changes ONLY if they WANT access to Draft Mode. Standard Prestige is available to non-addon users.**


---

## 📦 Repository Structure

```
/                               # Git root
├── Sql & Serverside Files/
│   ├── SQL/
│   │   ├── Acore_characters/
│   │   │   └── prestige.sql
│   │   └── acore_world/
│   │       ├── chromie_spawn.sql
│   │       ├── playercreateinfo_additions.sql
│   │       ├── playercreateinfo_action_additions.sql
│   │       ├── playercreateinfo_items_additions.sql
│   │       └── prestige_draft_specific_tables.sql
│   └── DBC/
│       └── *.dbc               ← Custom DBC overrides
├── Lua Files/
│   ├── Prestige & Draft Mode/
│   │   ├── prestige.lua
│   │   ├── prestige_chromie.lua
│   │   ├── spell_choice.lua
│   │   └── prestige_nameplates_hooks.lua
│   └── prestige_and_spell_choice_config.lua
├── Client Side Files/
│   ├── Client Addon/
│   │   └── PrestigeSystems/    ← WoW AddOn folder
│   └── MPQ Patch/
│       └── patch-P.mpq         ← Client data patch
└── README.md                   ← This file
```

---

## 🔧 Prerequisites

* **AzerothCore** server (3.3.5a/WotLK)
* **Eluna** Lua Engine enabled
* Access to **characters** and **world** MySQL databases
* A working WoW 3.3.5a client

---
(There are more novice friendly install instructions found in Install_Guide.txt)
## 🛠️ Server-Side Installation

Follow these steps on your server machine:

1. **Core Prestige Schema**

   ```sql
   -- characters DB
   SOURCE "Sql & Serverside Files/SQL/01_prestige.sql";
   ```

   Creates tables to track prestige and draft state.

2. **Optional Chromie NPC**

   ```sql
   -- world DB
   SOURCE "Sql & Serverside Files/SQL/02_chromie_spawn.sql";
   ```

   Registers NPC (entry 2069426) for a Chromie interface.

3. **Character-Creation Templates**

   ```sql
   -- world DB
   SOURCE "Sql & Serverside Files/SQL/03_playercreateinfo_additions.sql";
   SOURCE "Sql & Serverside Files/SQL/04_playercreateinfo_action_additions.sql";
   SOURCE "Sql & Serverside Files/SQL/05_playercreateinfo_items_additions.sql";
   ```

   *These are **`INSERT IGNORE`** scripts—existing data is preserved.*

4. **Draft-Specific Tables**

   ```sql
   -- world DB
   SOURCE "Sql & Serverside Files/SQL/06_prestige_draft_specific_tables.sql";
   SOURCE "Sql & Serverside Files/SQL/07_Professions_Allow_Patch.sql";
   ```

   Adds read-only tables used exclusively by the mod.

5. **DBC Overrides**

   ```bash
   cp "Sql & Serverside Files/DBC/*.dbc" "/path/to/server/Data/dbc/"
   ```

   Place custom DBC files into your server's `Data/dbc/` folder.

6. **Lua Scripts**

   ```bash
   cp -r "Lua Files/Prestige & Draft Mode/" "/path/to/server/lua_scripts/"
   cp "Lua Files/prestige_and_spell_choice_config.lua" "/path/to/server/lua_scripts/"
   ```

   Final layout in `lua_scripts/`:

   ```
   lua_scripts/
   ├── Prestige & Draft Mode/
   └── prestige_and_spell_choice_config.lua
   ```

Your server is now configured for **Prestige & Draft Mode**.

---

## 🎮 Client-Side Installation

On each player's machine:

1. **AddOn Installation**

   ```bash
   cp -r "Client Side Files/Client Addon/PrestigeSystems/" "<WoW Path>/Interface/AddOns/"
   ```

2. **MPQ Patch**

   ```bash
   cp "Client Side Files/Mpq Patch/patch-P.mpq" "<WoW Path>/Data/"
   ```

Restart the client. The AddOn and data patch enable the in-game UI for drafting spells.

---

## 🚀 Usage

1. **Level a character** to max (configurable).
2. **Interact** with the Prestige NPC (or Chromie) to select between Standard and Draft Prestige.
3. **Enjoy** prestige progression!
4. **Option Available** to quit draft mode at any time and revert to normal prestige by speaking to Prestige NPC.
---

## 📜 Configuration

All adjustable parameters (NPC IDs, max level, rerolls, etc.) live in:

```
lua_scripts/prestige_and_spell_choice_config.lua
```

Edit this file before deployment to match your server’s design.

---
## ❓ Frequently Asked Questions

### Q: Where can I change the rarity of certain spells?

**A:** In  `acore_world.dbc_spells`(NOT to be confused with your servers spell_dbc), you will find a column marked `Rarity`. 

`Common = 0, Uncommon = 1, Rare = 2, Epic = 3, Legendary = 4, Broken = 5`

### Q: Do I need to patch my client?

**A:** Yes. The `patch-P.mpq` file enables visual assets and spell support for the Prestige & Draft system. Without it, core features like spell icons and the UI may not function correctly.

### Q: What happens to my gear when I prestige?

**A:** All equipped gear is unequipped and mailed back to you automatically.

### Q: What are the requirements to prestige?

**A:** You must be max level(configurable), have at least 10 free inventory slots, and have no active pet.

### Q: Is this Mod compatible with other systems?

**A:** The system is self-contained and should not interfere with existing character progression or other custom modules unless they conflict with player class or spell learning behavior.

### Q: I have replaced files in an attempt to update and now I'm getting database errors. What do I do?

**A:** This Mod is a work in progress and continues to evolve. Always check the included prestige.sql file for any update queries required to keep your database schema in sync with the latest version.

### Q: I don't want to use this mod anymore, how do i get rid of it?

**A:** Remove the associated scripts from your lua_scripts folder. Optional: Replace serverside dbc files with backups. Thats it.

## 🙏 Credits & License

* **Author**: Stephen Kania
* **License**: MIT License (see `LICENSE`)
* **Based on**: AzerothCore, TrinityCore, and Eluna

Happy Drafting!
