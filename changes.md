# Larmod Changes

- Purpose
  - Fix my main issues with Cashmod (which was still probably the best competitive version of the game we've had thus far)
  - Expirement with big changes (ie the stam overhaul)
- All changes are balanced for around 52 ping
- This doc compares this mods changes to cash mod

## Strike Chamber Window Slightly

- `customStrikeChamberWindow=0.200` -> `customStrikeChamberWindow=0.225`

- **Why**
  - Extra 25ms seems enough to make chamber a viable mechanic while still allowing relatively easy counter play
  - On cashmod, chamber window feels too tight
    - Sometimes chamber doesn't even catch accels
    - "Perfect parry" timing, while not giving any rewards other than initiative
    - With the downsides of chamber (higher stam cost, chftp stun, etc) it felt like chamber was never the correct option

## Feint Lockout Increased (can punish feints consistently)

- `StrikeAndStabFeintLockout=(X=0.1,Y=0.1,Z=0)` -> `StrikeAndStabFeintLockout=(X=0.125,Y=0.125,Z=0)`

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
    - Time in seconds that gets added to the end of `expirementalParryDuration`
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

## Increased Late Riposte Window (expiremental)

- `ConfigRiposteWindow=0.1` (unchanged)
- `ExperimentalParryDuration=0.01` -> `ExperimentalParryDuration=0.1`
- `SuccessfulParryBonusDuration=0.05` -> `SuccessfulParryBonusDuration=0.1`
- Kind of complicated and still not fully understood, but testing feels good so far.

- **Why**
  - Slight buff to ripostes. In cash mod, morph mixups seem to be the optimal way to play 99% of the time.
  - Even in testing with this change, morph mixups were just better.

## Movement Acceleration reduced (expiremental)

- `TimeToMaxSprint=3` -> `TimeToMaxSprint=4`

- **Why**

  - Makes the game feel less "arcadey"
  - You shouldn't have to be an aim god to hit people
  - Makes mispositions more punishable **if** the defender doesn't keep momentum properly

- **Downsides**
  - May make swapping and pulling more difficult (person being pulled has more time to react to you running off)

## Stamina System Overhaul (expiremental)

- `StaminaRegen=(X=2.0,Y=5,Z=0.25)` -> `StaminaRegen=(X=0.55,Y=3,Z=0.25)`
- `customChamberFeintCost=10` -> `customChamberFeintCost=15`
- In Weapon BPs

  - Feint cost `10` -> `15`
  - Morph cost `7` -> `10`
  - Miss cost `12` -> `20` (flat for all weapons)

- **Why**

  - Philosophy
    - Lose stam for offensive options (morph, feint, etc)
    - Lose stam for mistakes (missing)
    - Rewarded stam when you do something good (hits, reading, creating space)

- **Downsides**
  - May cause more "low stam shenanigans" where the person who is losing on stam has more ways to get the advantage back
