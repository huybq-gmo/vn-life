# Vietnam Life Sim

Godot 4.7.2 (Standard/GDScript) project. Phase 1 adds a playable character,
two camera modes, and a controls menu. Phase 2 adds an expanded 140x140
graybox neighborhood, seven interactive locations, destination guidance, and a
checkpoint at home. Phase 3 adds a rideable arcade-style bicycle from the
provided GLB asset.
Phase 4 adds delivery jobs, food purchases, a wallet and a transaction ledger.
Phase 5 adds Lan's schedule, dialogue, a shared walk and slow hunger/energy needs.
Phase 6 adds versioned offline progress saves and a fallback backup.
Phase 7 adds a local/LAN ENet server for shared player positions, vehicle,
deliveries and NPC relationships.

## Start

On this Ubuntu workstation, sign out and back in once after the `render` group
change, then run `godot --display-driver wayland --editor --path .` from this
directory. The Wayland driver uses the Intel GPU; the default X11 driver on
this workstation currently falls back to CPU rendering. Press F6 in
`scenes/main.tscn` to play the scene, or F5 to run the project. On other
machines, `godot --editor --path .` may be sufficient.

Use WASD to move, Shift to run, Space to jump, V to switch cameras and Esc
to open the controls menu. The menu lets you change the movement keys, mouse
sensitivity, inverted vertical look, and adult male/female appearance. Settings persist in Godot's
`user://controls.cfg`.
Use E near a location to interact. Use M to release the mouse and select a
destination from the top-right menu; selecting a location resumes movement.
Use E by the bicycle to mount, then WASD to accelerate, brake, and steer.
Stop before dismounting with E; use R to return a stopped or stuck bike to
its parking spot.
At the work point, press E to start a delivery, then collect it at the cafe
and deliver it to the shop. Outside a pickup, E at the cafe buys a meal.
Press B to view the wallet and transaction history.
The wallet's Milestones tab shows recorded life events.
Press `/` to enter a local emote command such as `/wave`, `/handshake`,
`/dance`, or `/sit`. Movement cancels an active emote.
Press E near Lan to talk or start a walk together. Stay near Lan and walk 18 m
to finish the activity; return home to rest.

Run the automated movement, collision, camera, and input-settings checks with
`godot --headless --path . --script res://tests/test_phase1.gd`.
Check the neighborhood and interaction flow with
`godot --headless --path . --script res://tests/test_phase2.gd`.
Test the bike with `godot --headless --path . --script res://tests/test_phase3.gd`.
Test deliveries and money with `godot --headless --path . --script res://tests/test_phase4.gd`.
Test Lan and needs with `godot --headless --path . --script res://tests/test_phase5.gd`.
Test save, restart and recovery with `godot --headless --path . --script res://tests/test_phase6.gd`.
Test the two character variants and milestone journal with
`godot --headless --path . --script res://tests/test_character_visual.gd`.
Run the two-client LAN integration tests (including disconnect/rejoin) with
`bash tests/run_phase7.sh`.
Offline progress is stored in Godot's `user://progress.json`; a corrupt or
incompatible save is preserved and will not be overwritten. The player
respawns safely at home; time offline does not age the character or charge fees.

## Local multiplayer (Phase 7)

Open three terminals in this project directory. First start the headless
server, then launch two game windows with different session names:

```bash
godot --headless --path . --script res://scripts/dedicated_server.gd
godot --display-driver wayland --path . -- --join=127.0.0.1 --session=player_a
godot --display-driver wayland --path . -- --join=127.0.0.1 --session=player_b
```

On this workstation, use `sg render -c 'godot ...'` for each graphical
command if the current login still lacks the `render` group. On other workstations,
omit `--display-driver wayland` if it is not supported. For a LAN peer,
replace `127.0.0.1` with the server's private IPv4 address; port 24680 is
the default. Do not forward that port to the Internet. Session names are
temporary identifiers without authentication, and server progress exists
only until the dedicated server restarts. Offline saves are separate from
LAN progress. Stop the server with Ctrl+C.

The Godot Tools extension enables editing GDScript in VS Code. To connect the
editor's language server to VS Code, open the project in Godot while keeping
this folder open in VS Code. Git and Docker are already present on the current
development machine. Account credentials and database infrastructure are not
needed for the LAN prototype.

The game design is in `vietnam-life-sim-game-planningv2.md`. The scoped build
phases and acceptance criteria are in `docs/ROADMAP.md`.
Implemented Phase 1-6 gameplay rules and known gaps are in
`docs/GAMEPLAY_PHASES_1_6.md`.
# vn-life
