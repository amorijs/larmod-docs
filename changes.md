# Larmod Changes

- Purpose
  - Fix my main issues with Cashmod (which was still probably the best competitive version of the game we've had thus far)
  - Experiment with big changes (ie the stam overhaul)
- All changes are balanced for around 52 ping
- This doc compares this mods changes to cash mod

## Increase Strike Chamber Window Slightly

- `customStrikeChamberWindow=0.200` -> `customStrikeChamberWindow=0.225`

- **Why**
  - Extra 25ms seems enough to make chamber a viable mechanic while still allowing relatively easy counter play
  - On cashmod, chamber window feels too tight
    - Sometimes chamber doesn't even catch accels
    - "Perfect parry" timing, while not giving any rewards other than initiative
    - With the downsides of chamber (higher stam cost, chftp stun, etc) it felt like chamber was never the correct option

## Feint Lockout Increased (can punish feints consistently) & Parry Recovery Increased

- `StrikeAndStabFeintLockout=(X=0.1,Y=0.1,Z=0)` -> `StrikeAndStabFeintLockout=(X=0.125,Y=0.125,Z=0)`
- `customParryRecoveryTime=0.775` -> `customParryRecoveryTime=0.800`

  - Parry recovery should scale evenly with feint recovery, hence the increase
  - This means true combo will be even stronger, but at least now it's a more consistent mechanic where sometimes you don't get true combo and sometimes you do. Now if someone parrys after they got hit, you can almost always true combo

- **Why**
  - Punishing feints feels good
  - On cashmod, feints were only consistently punishable with faster weapons (ie Evening Star stab). This makes it so feints should be punishable with any weapon.
  - Punishing feints still requires some skill and reaction speed, and the read obviously.

## Stab Curve Slowed Down Slightly

- Important to note, you are still getting hit at the **exact** same time from when the stab animation starts. This change just makes the forward motion more readable

- **Why**

  - Makes stab animation more digestible, and technically less strong, albeit slightly
  - Stab is still strong and difficult read at the target ping (52ms)

- **Downsides**
  - Nerfs neutral game (morph mixups). That being said, it still seems to be the best way to play and is still very hard to defend against at target ping

## Extended parry increased, Active parry removed

- For clarity, some definitions
  - Extended parry: How much time **after** your parry lands that you have "bonus" parry
  - Active parry: How much time **after** you start riposte that you have "bonus" parry. This overwrites your extended parry window.
- Implementation is a little more complicated than just changing extended parry value, since that messes with late ripostes
  - `ExperimentalParryDuration=0.01` -> `ExperimentalParryDuration=0.1`
    - Time in seconds that always gets added to end of your parry
  - `SuccessfulParryBonusDuration=0.1` (unchanged)
    - Time in seconds that gets added to the end of `iParryDuration`
    - This is the actual extended parry value, but since extended parry is directly tied to late ripostes, we use these 2 values to recreate a 200ms extended parry without messing with late ripostes. This has been tested and we found no differences or kinks in using this method. That being said, still needs more testing to be sure it is understood clearly.
  - `ActiveParryDuration=0.050` (50ms) -> `ActiveParryDuration=0`
- This means, whenever you are in riposte, you will **never** have your parry window up

- **Why**

  - Extended parry increased
    - It sucks, and is incredibly frustrating when you are in 1vX and you tried to defend 2 attacks but were hard blocked by the game because the timings were synced up. Defender now has a reasonable option for defending 2 attacks coming at very tight timings. They can make plays around the 200ms window.
  - Active parry removed
    - When the you have swapped to offense (ie riposte), you should no longer have "free" parries
    - Think of buckler or targe on vanilla, it's quite lame that the defender gets free parries during their riposte. This is the same issue but less exaggerated.
    - This makes it so you can always hit someone if they are in riposte, no mental math required
    - This gives riposting in 1vX a more clear trade off. You are no longer invulnerable for x amount of time

- **Downsides**
  - _May_ see more "backparries" (I don't think this is the case, but needs testing.)
  - Technically this does make people harder to kill for mispositions. I do subscribe to the idea that people being easy to kill makes teamfight more nuanced and increases variety of macro. That being said I think we should strive to make 1vX extremely difficult, and not impossible in some situations.

## Increased Late Riposte Window (experimental)

- `ConfigRiposteWindow=0.1` (unchanged)
- `ExperimentalParryDuration=0.01` -> `ExperimentalParryDuration=0.1`
- `SuccessfulParryBonusDuration=0.05` -> `SuccessfulParryBonusDuration=0.1`
- Kind of complicated and still not fully understood, but testing feels good so far.

- **Why**
  - Slight buff to ripostes. In cash mod, morph mixups seem to be the optimal way to play 99% of the time.
  - Even in testing with this change, morph mixups were just better.

## Movement Acceleration reduced (experimental)

- `TimeToMaxSprint=3` -> `TimeToMaxSprint=4`

- **Why**

  - Makes the game feel less "arcadey"
  - You shouldn't have to be an aim god to hit people
  - Makes mispositions more punishable **if** the defender doesn't keep momentum properly

- **Downsides**
  - May make swapping and pulling more difficult (person being pulled has more time to react to you running off)

## Stamina System Overhaul (experimental)

This is by far the biggest change, and still needs heavy testing. The philosophy of the new stam system is, lose stam much faster, but also gain it back much quicker. Hopefully this makes for more dynamic play, but we will have to see.

- `StaminaRegen=(X=2.0,Y=5,Z=0.25)` -> `StaminaRegen=(X=0.55,Y=3,Z=0.25)`

  - x = Time in **seconds** to start stam regen
  - y = how much stam you get in a **tick**
  - z = how often in **seconds** a tick happens
  - Equation for stam regen per second: `y * z * (1 / z)`
    - ie `3 * 0.25 * (1 / 0.25)` = _12 stam per second_

- `customChamberFeintCost=10` -> `customChamberFeintCost=15`
- In Weapon BPs

  - Feint cost `10` -> `15`
  - Morph cost `7` -> `10`
  - Miss cost `12` -> `20` (flat for all weapons)

- **Why**

  - Lose stam for offensive options (morph, feint, etc)
  - Lose stam for mistakes (missing)
  - Rewarded stam when you do something good (hits, reading, creating space)
  - Immediate reward when your opponent dumps stam (ie feint spam)

- **Downsides**
  - May cause more "low stam shenanigans" where the person who is losing on stam has more ways to get the advantage back
  - Players may not be used to how fast stam goes, especially if they're missing or constantly buffering. The game plays much differently so test with an open mind.

## Stab Chamber Cost Reduced (experimental)

- `StabChamberStamCost=15` -> `StabChamberStamCost=10`

## CHFTP Stun Removed (experimental)

- `useChftpStun=1` -> `useChftpStun=0`

## Necessary Evil - True combo

- Ideally, there is no true combo. But the simplest fix for this is to reintroduce some form of miss detector (ie when you get miss detector speed up parry recovery by 100ms). This has an obviously bad smell to it, because having any miss detector in a team fight environment introduces inconsistencies. Even if the miss detector effect wasn't perceivable to the player, it could change outcomes.
- The only other way to fix this at the moment, is to change a ton of timings (ie parry recovery, miss combo speed, etc). This opens up a can of worms that we don't want to deal with at the moment.
