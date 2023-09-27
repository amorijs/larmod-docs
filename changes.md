# Larmod Changes

- This doc won't always have the most up to date changes as I experiment with things a lot. Sorry in advance if something isn't 100% accurate.
- Purpose
  - Fix my main issues with Cashmod (which was still probably the best competitive version of the game we've had thus far)
  - Experiment with big changes (ie the stam overhaul)
  - Consistent mechanics
  - A game that _feels_ more fun to play
- All changes are balanced for around 48-52 ping
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

## Stab Curve Slowed Down to Vanilla

- Still testing the vanilla stab curve, versus a "halfway" between vanilla and cash mod. but at the target ping, trying to reliably read a stab vs a stab morph is still very hard, even with the vanilla stab curve
- Important to note, you are still getting hit at the **exact** same time (while in facehug) from when the stab animation starts. This change just makes the forward motion more readable

- **Why**

  - Makes stab animation more digestible, and technically less strong, albeit slightly
  - Stab is still strong and difficult read at the target ping (48ms)

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

## Movement Acceleration reduced

- `TimeToMaxSprint=3` -> `TimeToMaxSprint=4`

- **Why**

  - Makes the game feel less "arcadey"
  - You shouldn't have to be an aim god to hit people
  - Makes mispositions more punishable **if** the defender doesn't keep momentum properly

- **Downsides**
  - May make swapping and pulling more difficult (person being pulled has more time to react to you running off)

## Stamina System Overhaul (experimental)

This is by far the biggest change, and still needs testing. The philosophy of the new stam system is, lose stam much faster, but also gain it back much quicker. Hopefully this makes for more dynamic play, but we will have to see.

- `StaminaRegen=(X=2.0,Y=5,Z=0.25)` -> `StaminaRegen=(X=0.5,Y=3.5,Z=0.25)`

  - x = Time in **seconds** to start stam regen
  - y = how much stam you get in a **tick**
  - z = how often in **seconds** a tick happens
  - Equation for stam regen per second: `y * z * (1 / z)`
    - ie `3 * 0.25 * (1 / 0.25)` = _12 stam per second_

- `customChamberFeintCost=10` -> `customChamberFeintCost=15`
- In Weapon BPs

  - Feint cost `10` -> `15`
  - Morph cost `7` -> `12`
  - Miss cost `12` -> `18`
  - Hit reward `10` -> `15`
  - `StabChamberStamCost=15` -> `StabChamberStamCost=10`

- **Why**

  - Lose stam for offensive options (morph, feint, etc)
  - Lose (a lot) stam for mistakes (missing)
  - Rewarded stam when you do something good (hits, reading, creating space)
  - Immediate reward when your opponent dumps stam (ie feint spam)

- **Downsides**
  - May cause more "low stam shenanigans" where the person who is losing on stam has more ways to get the advantage back
  - Players may not be used to how fast stam goes, especially if they're missing or constantly buffering. The game plays much differently so test with an open mind.

## No Parry During CHFTP Stun

`useChftpStun=1
DisableParryInChftpStun=1
ChftpStunParryDuration=0
CustomChftpStunStamCost=0.0
ChftpStunParryRecoveryTime=1.5
ChftpStunTurnCap=3
ExcludeStabChftpStam=1
ChftpStunValues=(X=1.25,Y=2,Z=1)`

- ~~Due to current limitations, you have a 1ms parry window during your stun animation. So unless you are hitting the god parry, you are getting hit (if the opponent punishes on time)~~ - Fixed!
- ~~This also means the current meta is to immediately parry after your CHFTP so you can actually moved. Ideally, there is no stun, you are just in parry recovery but can move freely, but again, need access to the stun BP to make this possible.~~ - Fixed!

- **Why CHFTP stun**

  - See the "Neccessary Evil" section below

- **Why force damage on CHFTP**

  Again, every offensive counter to a defensive option results in damage on the defender for failing. It should be no different here. There is no logical reason to be put into another 50/50 (timing parry for accel or drag/feint) after already failing defensively.

  Ideally, when you CHFTP, it just puts you in parry recovery (you can move, but not parry) instead of stunning you, but we can't do that at the moment. Cswic is working on making the stun BPs accessible for us so hopefully we can make it happen soon

## Kick Unfeintable

- **Why**
  - Kick feint runaway mixup is really silly, and puts too much onus on the attacker to make the perfect spacing play
    - If I go in, I get kicked
    - If I don't go in, they dodge me
    - I can counter kick after going in, but now I have to burn stam, and risk giving up initiative because my opponent is doing this mix up on me
  - Kick is the only attack in the game that breaks timings and can "steal" initiative
    - In this system, kick is commital, and a risk, but still a viable option

## Kick Recovery is Faster

- Felt appropriate considering the nerf
- Still punishable with timing / reacting as soon as they start their animation
- Experimental, needs more testing

## Kick Miss is 10 Stam

Felt appropriate considering the nerf

## Dagger, Rapier, Scimitar stab windup slowed down

Must I explain why

## Max FOV Increased to 115

Common request, not really opinionated on this one

## Increased Recovery After Hitting Teammate

No more true comboing off a teamhit. Recovery is same as parry recovery now

## Miss Recovery Increased by 200 (max 900ms)

When your opponent misses, you now have initiative (very fast weapons can sometimes beat you)

## Miss Combo Removed (Experimental)

Missing is a mistake, or at the least a trade off (2v1s). IMO, there is no reason to have the one of the strongest attacks in the game (combo) that you can chain off a miss. This is one of those changes that feels so much better to us in testing. The miss combo whirlwind sucks to be on the receiving end of.

## 2HTK Generally Removed

Classic 2htk weapons now do 49 to body, and 65+ to head. This means they synergize both ways, headshot/bodyshot, with most weapons. Maul unchanged

ex: exec to head does 65, GS to body does 36

ex2: GS does 51 to head, exec does 49 to body

## Executioner Sword & Polehammer can Combo (Experimental)

Now that they aren't true 2htk, it felt completely irrelevant. Combo felt reasonable here.

## ES 75 to Head, Polehammer 90 to Head, Exec 65 to Head (Experimental)

This is to allow them to synergize with most weapons body damage

Polehammer might be a bit too overtuned, but just wanted to test an extreme. In testing it didn't feel overpowered, especially since slower weapons don't have strong feints

## Feint Window & Morph Window Decreased Slightly (Experimental)

Most feints without actually using your body to make the animations scary are readable. A good feint is still unreadable. For some reason, overhead feints are pretty readable regardless of what you do to make anims scarier, this is probably because you can't hide the weapon behind you (like looking up with an lmb). Fast weapons still have very strong feints (BS, poleaxe, etc)

Morphs are less of a offensive tool and more of a utility to increase windup and maybe catch people off guard in teamfight. That being said, good morphs (using camera movement) are still mostly unreadable especially with faster weapons.

## Necessary Evil - True combo

Ideally, there is no true combo. But the simplest fix for this is to reintroduce some form of miss detector (ie when you get miss detector speed up parry recovery by 100ms). This has an obviously bad smell to it, because having any miss detector in a team fight environment introduces inconsistencies. Even if the miss detector effect wasn't perceivable to the player, it could change outcomes.

The only other way to fix this at the moment, is to change a ton of timings (ie parry recovery, miss combo speed, etc). This opens up a can of worms that we don't want to deal with at the moment.

## Necessary Evil - CHFTP Stun

In a world where CHFTP stun doesn't exist:

- Feint counters parry
- Parry counters nuetral accel and neutral drag (usually)
- Chamber counters feint and accel, and can parry out if it's a drag

Do you see the problem here? There is no counter to chamber because the defender has a safety net (feint to parry) if the chamber doesn't catch.

You might say, "well they can just take a lot of stam punish for CHFTPing", but any form of punishment where the defender isn't getting hit for failing, is probably a bad punishment. For example, without CHFTP stun, I can decide to just burn all my stam to become temporarily invincible and delay in a teamfight.

There's an argument to be made that the player should be able to use their stam resource as a "perfect" defense option, but I don't agree with it at all. IMO, there should always be a way for the attacker to do damage if they outplay the defender.

Even though I think CHFTP stun isn't the ideal way to solve the issue, it's the only way we can at the moment, while keeping chamber in the game. It's a neccessary bandaid imo.
