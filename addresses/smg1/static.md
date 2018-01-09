# Static-address values

These values aren't identified as being part of a particular memory "block", but they can all be found based on the game start address (start address + offset).


## Items

Item | Offset | Bytes | Format | Notes | Versions confirmed
--- | --- | --- | --- | --- | ---
Button statuses 1 | 0x61D342 | 1 | Binary | Bit order: Home, C, Z, -, A, B, 1, 2. Up to 8 other addresses might work, but this seems to be the earliest address that works. | NA JP
Button statuses 2 | 0x61D343 | 1 | Binary | Bit order: ?, ?, ?, +, D-pad Up, Down, Right, Left. | NA JP
Wiimote shake input related | 0x61D34C | 20 | ?
Stick X input | 0x61D3A0 | 4 | Float | 1.0 all the way right, -1.0 all the way left, 0.0 neutral. Corresponds to the limits recognized by the game, not the limits of the physical stick, so you don't have to push to the limit to get +/- 1.0
Stick Y input | 0x61D3A4 | 4 | Float | 1.0 all the way up, -1.0 all the way down, 0.0 neutral
Nunchuk shake input related | 0x61D3A8 | 20 | ?
Star bit count (in level) | 0x91F7CC | 4 | Integer |  | NA JP
Scene you're about to enter | 0x9A9168 | 4 | Integer | 1 in Terrace load zone, 2 for Fountain, 3 for Kitchen, etc. 0xFFFFFFFF when invalid. Goes to valid values as the screen begins to fade to black, then 0xFFFFFFFF before it finishes going to black. From MutantsAbyss | NA JP
Stage time, frames | 0x9ADE58 | 4 | Integer | Starts after fading in from star select. Does not stop when you get the star. Resets to 0 if you die. | NA JP
Number of lives | 0xF63CF1 NA, 0xF63F49 JP | 1 | Integer | NA address is from "99 Lives [dexter0]" on geckocodes. JP address was found based on that | NA JP
Current area | 0xFB36C4 NA, 0xFB3A64 JP | 4 | Integer | 1 in the Observatory, 8 in the domes, 8 on the title screen. Also possibly at 0xFB3878 and 0xFB39C8 (JP). The address listed here has a value of 0x15346 in Good Egg 1. From MutantsAbyss | NA JP
