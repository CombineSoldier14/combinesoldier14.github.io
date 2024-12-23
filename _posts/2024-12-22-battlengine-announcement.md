---
layout: post
title: Merry Christmas - Your gift is BattleEngine!
---

Hello everyone! It's Christmas time, and I've started learning C++. Contrary to popular belief, it's actually very fun and I love it. (except CMake and learning build systems, fuck that). So because of my newfound interest and the holidays, I've got a new coding project for you all, and that is **BattleEngine**!

BattleEngine is a highly customizable turn-based battle game written in C++. You can use a `settings.json` to change values like names, amount of healing potions/minions, and soon, customize and make your own attacks. It's similar to Pokemon or other RPGs.

```
Made with BattleEngine v1.3.2 by CombineSoldier14
-------------------------------------------------------
The battle has begun!
John vs Tim
-------------------------------------------------------
Current Turn: John
John's health: 200
Tim's health: 200

Attacks:
1. Healing Potion
2. Large Attack
3. Small Attack
4. Summon Minion

Healing Potions left: 5
Available Minions: 2
Minions add a random damage boost (potentially double) but lower your chances of hitting. They last for 5 of your turns.
Minion Active?: No
-------------------------------------------------------
Type the name of your attack.
> 
```

```
Made with BattleEngine v1.3.2 by CombineSoldier14
-------------------------------------------------------
The battle has begun!
Phoenix Wright vs Miles Edgeworth
-------------------------------------------------------
Current Turn: Phoenix Wright
Phoenix Wright's health: 200
Miles Edgeworth's health: 200

Attacks:
1. Healing Potion
2. Objection!
3. Hold it!
4. Summon Minion

Healing Potions left: 3
Available Minions: 4
Minions add a random damage boost (potentially double) but lower your chances of hitting. They last for 5 of your turns.
Minion Active?: No
-------------------------------------------------------
Type the name of your attack.
> 
```

```
Made with BattleEngine v1.3.2 by CombineSoldier14
-------------------------------------------------------
The battle has begun!
Sonic vs Mario
-------------------------------------------------------
Current Turn: Sonic
Sonic's health: 100
Mario's health: 100

Attacks:
1. 1-up
2. Super Sonic
3. Spindash
4. Summon Minion

1-ups left: 4
Available Minions: 5
Minions add a random damage boost (potentially double) but lower your chances of hitting. They last for 5 of your turns.
Minion Active?: No
-------------------------------------------------------
Type the name of your attack.
>
```

The attacks use RNG by default (though once you can customize attacks in a future update this can be changed too). You also might notice this is based off of CombineBot's battle command (actually this began as a hobby port of that to C++)

I've had a lot of fun with this and I hope you can check it out too!

https://github.com/CombineSoldier14/cpp-projects
(under `battle-engine`)
