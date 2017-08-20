# Spin related values


## Shake status

**Wiimote shake bit 1**: Start + 0x9E9BE8 (1 byte integer)

  * 1 on a frame where the game registers a Wiimote shake, 0 otherwise.
  * If you hold Shake X/Y/Z on a Dolphin emulated Wiimote, this value is 1 for about 75% of frames.
  * On the ground or in midair, if there is no spin cooldown or suppression, a spin occurs when this bit goes from 0 to 1.
  * In water, if Wiimote continuous shake time 1 is not running, a spin occurs when this bit goes to 1.
  
**Wiimote shake bit 2**: Start + 0x9E9BFC (1 byte integer)

  * Similar behavior to Wiimote shake bit 1, but the value is 1 on different frames.
  * This bit does not seem to trigger spins directly. However, it controls Wiimote continuous shake time 2, which in turn controls consecutive water spins.

**Nunchuk shake bit**: refPointer + 0x27F1 (1 byte integer)

  * 1 on a frame where the game registers a Nunchuk shake, 0 otherwise.
  * If you have Shake X/Y/Z on a Dolphin emulated Wiimote mapped to a button, you only get 1s when you press down or release this button. You can't autofire 1s.
    * On a frame by frame basis, holding this Shake input for 5 frames seems to get a Nunchuk shake pretty consistently.
  * If there is no spin cooldown, suppression, or active water spin timer, a spin occurs when this bit goes to 1.
    * It's unknown whether it has to go from 0 to 1 on that particular frame to trigger the spin. Having consecutive 1 frames is difficult with Nunchuk shakes, at least on Dolphin with an emulated controller.


## Wiimote water spin buffering

In order to do consecutive water spins on the earliest possible frame, you don't have to do every shake frame-perfectly. There is a Wiimote shake buffering mechanism. As long as you keep shaking with no big gaps (roughly 10 frames) between shakes, the next spin will come out on the earliest possible frame.

This buffering only works for Wiimote underwater spins and Wiimote surface spins. However, the memory values involved are active during all types of Wiimote spins.

The following memory values are involved:
  
**Wiimote shake downtime 1**: Start + 0x9E9BF2 (2 byte integer)

**Wiimote shake downtime 2**: Start + 0x9E9C06 (2 byte integer)

  * Both downtimes count up 1 per frame in the absence of Wiimote shakes.
  * Both go to 0 when you do a Wiimote shake, then resume counting up some frames afterward.
  * The shake sensitivity is a little different between the two downtime values. The details of the sensitivity difference are unknown; all we know is that the two downtimes often reset to 0 on different frames, and resume counting on different frames.
  * Active even at times when you couldn't normally spin, such as during the penguin race "3 2 1" and during a ground pound.

**Wiimote continuous shake time 1**: Start + 0x9E9BEE (2 byte integer)

**Wiimote continuous shake time 2**: Start + 0x9E9C02 (2 byte integer)

  * Both times are 0 in the absence of Wiimote shakes.
  * Both start counting up 1 per frame when a Wiimote shake happens.
  * Continuous shake time 1 resets to 0 when downtime 1 reaches 7.
  * Continuous shake time 2 resets to 0 when downtime 2 reaches 9.
  * In water, if there is no spin cooldown, suppression, or active water spin timer, a spin happens when:
    * Continuous shake time 1's value is 2, OR
    * Continuous shake time 2's value is 31, 46, 61, 76, etc. (intervals of 15 frames).
  * Active even at times when you couldn't normally spin.

**Wiimote water spin filter**: refPointer + 0x27F0 (1 byte integer)
  
  * In water: If you keep shaking the Wiimote, is 1 once every 15 frames, and 0 on other frames. 0 in the absence of Wiimote shaking.
    * The frames when this is 1 are the same frames when continuous shake time 2 is 31, 46, 61, 76, etc. So, 1 frames are the only frames when water spins can happen.
  * On ground and in midair: Behaves the same as Wiimote shake bit 1.

**Water spin timer**: refPointer + 0x6A4D (1 byte integer)

  * 29 on the second frame of an underwater spin, then counts down 1 per frame until it reaches 0.
    * Perfect consecutive spins make the timer go from the first frame of 0 to 29 at the start of the next spin.
    * If the spin bonks something, resets to 0.
  * 44 on the second frame of a surface spin, then counts down 1 per frame until it reaches 0.
    * Perfect consecutive spins make the timer go from the first frame of 0 to 44 at the start of the next spin.
  * 0 when inactive. Does not respond to A button swimming strokes or Z button dives. Does not respond to ground or midair spins.
  * When a spin goes out of the water, counting down stops and the previous value is retained.


## Spin suppression

There are two timers which seem to suppress spins from happening as long as they are running.

**Wiimote spin suppression timer**: refPointer + 0x27ED (1 byte integer)
  
  * Press A or B anywhere: Goes to 9, then counts down 1 per frame. During these countdown frames, Wiimote spins seem to be suppressed.
  * Ground spins: Goes to 8 when Wiimote shake bit is 1. Counts down 1 per frame until 0 when Wiimote shake bit is 0.
  * Midair spins: If this is 0 and Wiimote shake bit is 1, goes to 8, then counts down 1 per frame until 0. In midair, if a countdown starts, the value can't go to 8 again until the countdown finishes.
  * Does not respond to water spins.

**Nunchuk spin suppression timer**: refPointer + 0x27EF (1 byte integer)
  
  * Press Z or C anywhere: Goes to 9, then counts down 1 per frame. During these countdown frames, Nunchuk spins seem to be suppressed.
  * Ground spins: Goes to 8 when Nunchuk shake bit is 1. Counts down 1 per frame until 0 when Nunchuk shake bit is 0.
  * Midair spins: If this is 0 and Nunchuk shake bit is 1, SOMETIMES goes to 8, then counts down 1 per frame until 0. In midair, if a countdown starts, the value can't go to 8 again until the countdown finishes.
  * Does not respond to water spins, or pressing A or B.


## Other values

**Wiimote shake input related**: Start + 0x61D34C (20 bytes)

**Nunchuk shake input related**: Start + 0x61D3A8 (20 bytes)

**Spin animation bit**: refPointer + 0x1CB5, refPointer + 0x1CB6 (1 byte integer)

  * 1 during the spin animation, 0 otherwise.
    * Goes to 1 for ground, midair, water, Wiimote, and Nunchuk spins.
    * Does not go to 1 for mini midair spins (after the first midair spin), or throwing shells.
    * If you interrupt a ground spin with a jump, or interrupt a midair spin with a spin pound, goes to 0.
  * No known differences between the two addresses.

**Spin hitbox timer**: refPointer + 0x2214 (1 byte integer)

  * Seems to be the number of frames left in the current spin hitbox, though this has not been confirmed.
  * Is active for ground, midair, mini midair, water, Wiimote, and Nunchuk spins.
  * Is not active when throwing shells.
  * 25 at the start of a ground or midair spin, then counts down by 1 per frame until it reaches 0.
  * 15 at the start of a mini midair spin, then counts down by 1 per frame until it reaches 0.
  * 40 at the start of an underwater spin, then 30 next frame, then counts down by 1 per frame until it reaches 0.
    * Perfect consecutive underwater spins make the timer go from 2 to 40 at the start of the next spin.
  * 40 at the start of a water surface spin, then 45 next frame, then counts down by 1 per frame until it reaches 0.
    * Perfect consecutive surface spins make the timer go from 2 to 40 at the start of the next spin.
  * Does not reset to 0 if you interrupt a ground spin with a jump, or interrupt a midair spin with a spin pound.

**Consecutive spin hitbox time**: refPointer + 0x2215 (1 byte integer)

  * When the spin hitbox becomes active (i.e. that timer becomes non-0), this value counts up 1 per frame starting from 1.
  * When the spin hitbox becomes inactive, this addresses's up-counting stops. The value is retained until the spin hitbox is active again.

**Ground spin cooldown timer**: refPointer + 0x2217 (1 byte integer)

  * 79 at the start of a ground spin, then counts down by 1 per frame until it reaches 0.
    * Perfect consecutive spins make the timer go from 1 to 79 at the start of the next spin.
    * If you interrupt the spin with a jump, goes to 0.
  * 16 after landing from a midair spin, unless you ground pound.
  * In midair: Almost always 0, but is 79 for one frame at the start of a spin or mini spin.
  * Not active underwater.

**Time since last spin cooldown**: refPointer + 0x221A (2 byte integer)

  * Counts up 1 per frame in the absence of spins.
  * 0 when either the Wiimote shake bit or Nunchuk shake bit are 1.
  * 0 during a ground spin.
    * If you interrupt a ground spin with a jump, starts counting up again.
  * 0 during ground/midair spin cooldown.
  * 0 after a midair spin ends, until landing.

**Spin cooldown Luma animation timer**: refPointer + 0x221D (1 byte integer)

  * During spin cooldown, when the Luma-returning animation begins, this becomes 19, then counts down 1 per frame. 0 otherwise.
  * The beginning and end of the 19 frame countdown are close to the time that the Luma is visible. The beginning is just 1 frame early.
  * The end of the 19 frame countdown is 6 frames later than the end of the actual spin cooldown. So, it seems this animation timer has no bearing on the ability to spin.

**Midair spin timer**: refPointer + 0x41BF (1 byte integer)

  * 180 when inactive.
  * 22 at the start of a midair spin, then counts down 1 per frame. Goes from 1 to 180.
  * Does not respond to ground spins, water spins, or mini midair spins.

**Midair spin type**: refPointer + 0x41E7 (1 byte integer)

  * Goes to 0 when you jump.
  * Goes to 1 upon a midair Wiimote spin, 2 upon a midair Nunchuk spin. Does not respond to mini midair spins.
  * Remains unchanged on the ground and in the water.
  * Does not go to 0 if you walk off a ledge or bounce off an enemy.

**Water animation timer**: refPointer + 0x69FD (1 byte integer)

  * 32 on the second frame of a water spin, then counts down 1 per frame until it reaches 0.
  * 59 at the start of an underwater A button swimming stroke, then counts down 1 per frame until it reaches 0.
    * Does not respond to water surface A button strokes.
  * 14 at the start of a Z button water dive (the start of the actual diving movement, not the somersault), then counts down 1 per frame until it reaches 0.
  * 15 on a normal water entry, then counts down 1 per frame until it reaches 0.
  * 30 on a pound water entry, then counts down 1 per frame until it reaches 0.
  * 0 when inactive.
  * Does not respond to ground or midair spins.

**Unknown water timer**: refPointer + 0x6A01 (1 byte integer)

  * 49 on the second frame of an underwater spin, surface spin, or an underwater A button swimming stroke. Then counts down 1 per frame until it reaches 0.
  * When a spin goes out of the water, counting down stops and the previous value is retained.
  * Does not respond to Z button dives, or water surface A button strokes.
  * 0 when inactive.
  * Does not respond to ground or midair spins.
