- write ammo instruction address: 004C73EF (`ac_client.exe + C73EF`)
	-  to find the instruction for the recoil, I used this address and "stepped over" until I found it. 
- address: 0x00400000+0x0017E0A8 = 0057E0A8
- view angle address: 0x0057E0A8 + 0x38
- "ac_client.exe" address - 00400000
- you can "nop" the instruction at the address `004C73EF`: so the ammo doesn't decrement (infinite ammo for `EVERYONE`)
	- you can also do these for health, armor
	- i think this is called by a function that the player and the bots use (decrement ammo)

-  0057E0A8  00 60 7E 00 (the pointer at the address "0057E0A8" stores the address "007E6000" little endian representation)

- `ac_client.exe + 1C223` - mem address of instruction take executes when you take damage from bullets? (nvm, nobody takes damage, when nop-ed)
- `ac_client.exe + 1C223` - instruction that executes when player takes damage from grenades (no)

- `004C2EC3` - mem address of instruction that SHOULD change the pitch when shooting (recoil)
	- the instruction from the address memory of `004C2EC3` is 4 bytes long, xdbg replaced the instruction and next 4 bytes with `nop` 
-  the byte `0x90` represents the instruction `nop`
-  z coordonate of the player (`0x00400000+0x0017E0A8`)
-  `0x008569D8`
## TODO
-  disable the bullet spread
-  disable the recoil force/knockback whenever shooting
-  nice exercise: disable limit on grenades (you can pick as many grenades as you want)
-  shoot through walls (wallhack) - Edi