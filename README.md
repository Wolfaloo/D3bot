# AzBot
A very primitive AI for GMod bots primarily designed to work with Jetboom's Zombie Survival gamemode.

AzBot uses A* pathfinding with directed Monte Carlo-based execution.

# License
I, the author, have not decided on a license yet.

Though I won't take measures against illegimate usage unless I have reasons to do so.
##### Things I'll accept:
- Using AzBot on your own server even if it accepts donations.
- Making derivatives (aka copies with changes) of AzBot or its derivatives as long as it's made clear at least ingame that they're AzBot derivatives.
  
  I suggest giving the derivative your own name (e.g. BobBot) and putting e.g. "BobBot is based on AzBot" in your MotD, GUI, HUD or automatic chat messages.

##### Things I won't like:
- Selling AzBot or its derivatives (I don't plan to sell it either).

# Prerequisites
- Zombie Survival gamemode
- Multiplayer (bots use player slots, local server is sufficient)
- ULX (navmesh editor, !botmod and !human commands)
- NavMeshes (see "How to create navmeshes" below)

# How to create navmeshes:
- Use the console command "azbot editmesh \<your name\>" to enter the editor.
  - Use IN_RELOAD to cycle through the edit modes:
    - Create node: Place nodes with IN_ATTACK.
    - Link nodes: Link nodes by selecting the first then the second node, both with IN_ATTACK. Clear selection with IN_RELOAD.
    - Reposition nodes: Select a node with IN_ATTACK and use IN_ATTACK to reposition it or IN_ATTACK2 to reposition it using only the aim axis (X, Y or Z). Clear selection with IN_RELOAD.
    - Resize nodes: Select a node with IN_ATTACK and use IN_ATTACK2 to resize it on the aim axis (X or Y). Clear selection with IN_RELOAD.
    - Copy nodes: Select nodes with IN_ATTACK and use IN_ATTACK2 to copy them offset towards the aim axis (first selected node and cursored position are used as reference for the offset distance).
    - Delete items: Delete a node or link with IN_ATTACK, clear node areas with IN_ATTACK2.
- Use "azbot setparam \<id\> \<name\> \<value\>" (example: azbot setparam 1 Jump Disabled) to set special parameters:
  - Node parameters:
    - Jump = Disabled: Bots won't jump if located in this node.
    - Jump = Always: Bots will always jump if located in this node.
    - Wall = Suicide: Bots suicide if trying to navigate towards this node higher than crouch-jumping height. Use this when respawn is the only way to get to that node.
    - Wall = Retarget: Same as Wall = Suicide but target is changed instead of suiciding. If no other targets are available, target remains the same. Use this for unreachable or low priority nodes.
    - See = Disabled: Bot does not approach target in straight line even if target is visible to him unless he is on the same node as the target. Use this on heightened nodes visible to, but not directly accessible from lower nodes.
- Use "azbot reloadmesh" to discard changes.
- Use "azbot savemesh" to save the changes to garrysmod/data/azbot/navmesh/map/\<mapname\>.txt.

### Notes:
- Restart the map after saving the mesh for the first time. Every map that has a navmesh at addon loading time is treated as a bot map (enabling the bot count director, !human command and survivor bonuses). Move the navmesh file if that effect is not desired.
- Use !botmod to change the desired zombies count. Examples: !botmod -100 for no bots, !botmod 100 for full server minus 2 slots for joining players, default is !botmod 1.
- Navmeshes can be edited on the fly. Feel free to fix your meshes during testing.
- Having sized nodes helps locating (= "what node are you in?"). Unsized nodes use a small sphere instead of a rectangular area.
- Once a bot enters a node's area/sphere, he immediately moves towards the next node's position.
- Once a bot sees the target, he moves straight for it (see also: See = Disabled parameter).
- All links are bidirectional (see also: Wall = Suicide parameter).
- Bots can take any route with tendency towards the shortest.
- Bots stubbornly move towards the next node unless they enter another node at time of path refresh (occurs roughly every second). Link wisely.

# Current Project Status
Not being worked on. ETA for ToDo: +∞. Just bugfixes or dirty changes on contributor's demand.

# ToDo
Starting with highest public priority:
- Making a config.txt for the static variables.
- Refactoring, e.g. stable API, stable navmesh standard, consistency, bot metatable, gamemode independence by adding hooks usable by gamemodes or gamemode-based plugins, ...
- Detailed linking e.g. required jump height, movement behaviour (crouch, gap-jumping, ...), unidirectionality, link/node unlock conditions, ...
- Map information in navmeshes using a singleton item type solely for storing parameters (e.g. zombie count multiplier).
- Leap behaviour for headcrab and fast zombie bots.
- Sloped nodes for more accurate locating of entities.
- Triangle-based nodes using vertices with automatic adjacence linking.
- Subpaths in nodes for more accurate movement (no "wall-sliding").
- Shooting, escape and equipment upgrading behaviour for survivor bots.
- Caching of non-branching paths as a single node to optimize the pathfinding performance.
