# Static-address values

These values aren't identified as being part of a particular memory "block", but they can all be found based on the game start address.


## Items

Item | Offset | Bytes | Format | Notes
--- | --- | --- | --- | ---
Button statuses 1 | 0x61D342 | 1 | Binary | Bit order: Home, C, Z, -, A, B, 1, 2. Up to 8 other addresses might work, but this seems to be the earliest address that works. Works on NA and JP versions
Button statuses 2 | 0x61D343 | 1 | Binary | Bit order: ?, ?, ?, +, D-pad Up, Down, Right, Left. Works on NA and JP versions
Scene you're about to enter | 0x9A9168 | 4 | Integer | 1 in Terrace load zone, 2 for Fountain, 3 for Kitchen, etc. 0xFFFFFFFF when invalid. Goes to valid values as the screen begins to fade to black, then 0xFFFFFFFF before it finishes going to black. Works on NA and JP versions. From MutantsAbyss
Stage time, frames | 0x9ADE58 | 4 | Integer | Starts after fading in from star select. Does not stop when you get the star. Resets to 0 if you die. Works on NA and JP versions
Number of lives (A) | 0xF63CF1 NA, 0xF63F49 JP | 1 | Integer | NA address is from "99 Lives [dexter0]" on geckocodes. JP address was found based on that
Current area | 0xFB36C4 NA, 0xFB3A64 JP | 4 | Integer | 1 in the Observatory, 8 in the domes, 8 on the title screen. Also possibly at 0xFB3878 and 0xFB39C8 (JP). The address listed here has a value of 0x15346 in Good Egg 1. From MutantsAbyss
Number of lives (B) | 0x10844AC NA, 0x108484C JP | 4 | Integer | This is the only other address that works for tracking lives, even if you just stay in the Observatory
