# How to Pronounce Iwan: Reverse-Engineering Notes

## Overview

- Engine: Unity
- Game type: Arena combat
- Networking: No active networking has been confirmed
- Save location: Unknown

The assembly browser shows game classes including `ArenaControl`, `Explosion`, `Gun`, `Health`, `Poison`, `Robot_Gun`, `Robot_Move`, and `WeaponManager`.

![Game assembly and class list](image-5.png)

## Systems

### Weapons and combat

- The player selects weapons with a scroll-wheel weapon roulette.
- The player can carry up to three weapons and starts with none.
- `WeaponManager` handles weapon selection and switching. The screenshot shows weapon slots and a `Switch()` method.
- `Gun` contains weapon behavior, including a `Reload()` method that transfers ammunition between the current and reserve ammo values.
- Guns and enemy guns have separate classes.
- Combat takes place in an arena, and the game includes explosions.

![Weapon manager code](image-1.png)
![Gun reload code](image-2.png)

### Player health and damage

- The `Health` class tracks player health and damage values.
- Observed damage sources include bullets, melee, explosions, and poison.
- The screenshot shows initial health of 100, explosion damage of 4, melee damage of 1, and fire damage of 1. The bullet damage value is not initialized in the visible code.
- `Poison` checks for the `Player` tag and calls `Health.dead()`, which sets player health to zero. This appears to be an instant kill rather than gradual poison damage.

![Health class and damage values](image-3.png)
![Poison trigger behavior](image-4.png)

### Movement

- `CharacterMotor` manages player movement physics, including velocity, acceleration, and gravity.
- Related helper classes include `CharacterMotorMovement` and `CharacterMotorJumping`.

### User, achievements, and leaderboards

- User, achievement, and leaderboard systems are present or referenced in the game.
- A local leaderboard method exists, but `Local.ShowLeaderboardUI()` logs `ShowLeaderboardUI not implemented`.
- In the local playthrough, leaderboard points are not saved and the UI shows no leaderboard data.

![Local leaderboard method](image.png)

## Persistence and networking

- No active networking has been confirmed in the inspected gameplay code.
- The local leaderboard implementation is incomplete.
- Where user, achievement, and other game data are stored is unknown; do not assume it is saved locally until the save code or files are located.

## Open questions

- Where does the game save local user and achievement data, if anywhere?
- Are any networking or platform-service APIs called during normal gameplay?
- Is the three-weapon limit enforced by `WeaponManager` or by another component?
- How is bullet damage assigned, and which classes apply it?
- Is poison intended to kill immediately, or is the observed call a bug?
- Which enemy classes and arena systems control spawning and combat flow?