# Reference pointer based values

These values aren't identified as being part of a particular memory "block", but they can all be found based on the reference pointer.


## Items

Item | Offset | Bytes | Format | Notes
--- | --- | --- | --- | ---
Position | 0x18DC | 12 | Float 3D vector | Editable
Gravity down-vector | 0x1B10 | 12 | Float 3D vector
Life meter | 0x1C50 | 4 | Integer
Spin animation bit (A) | 0x1CB5 | 1 | Bit | [Spin related values](spin_related.md)
Spin animation bit (B) | 0x1CB6 | 1 | Bit | [Spin related values](spin_related.md)
Spin hitbox timer | 0x2214 | 1 | Integer | [Spin related values](spin_related.md)
Consecutive spin hitbox time | 0x2215 | 1 | Integer | [Spin related values](spin_related.md)
Ground spin cooldown timer | 0x2217 | 1 | Integer | [Spin related values](spin_related.md)
Time since last spin cooldown | 0x221A | 1 | Integer | [Spin related values](spin_related.md)
Spin cooldown Luma animation timer | 0x221D | 1 | Integer | [Spin related values](spin_related.md)
Wiimote spin suppression timer | 0x27ED | 1 | Integer | [Spin related values](spin_related.md)
Nunchuk spin suppression timer | 0x27EF | 1 | Integer | [Spin related values](spin_related.md)
Wiimote water spin filter | 0x27F0 | 1 | Integer | [Spin related values](spin_related.md)
Nunchuk shake bit | 0x27F1 | 1 | Bit | [Spin related values](spin_related.md)
General state | 0x3DC4 | 4 | Binary
Position, 1 frame late | 0x3EEC | 12 | Float 3D vector | Not editable. This value should be in sync with what's shown on the game screen
Base velocity | 0x3F64 | 12 | Float 3D vector | Not all kinds of movement are covered. For example, launch stars and riding moving platforms aren't accounted for
Tilt up-vector | 0x3FAC | 12 | Float 3D vector
Midair spin timer | 0x41BF | 1 | Integer | [Spin related values](spin_related.md)
Midair spin type | 0x41E7 | 1 | Integer | [Spin related values](spin_related.md)
Water animation timer | 0x69FD | 1 | Integer | [Spin related values](spin_related.md)
Unknown water timer | 0x6A01 | 1 | Integer | [Spin related values](spin_related.md)
Gravity up-vector | 0x6A3C | 12 | Float 3D vector
Water spin timer | 0x6A4D | 1 | Integer | [Spin related values](spin_related.md)
Water state | 0x6A6F | 1 | Integer | 0: on surface, not moving; 1: on surface, moving; 2: underwater, moving; 3: underwater, not moving; 255: out of water. The 'being pulled toward surface' movement state still counts as underwater
Air | 0x6AB8 | 4 | Integer | Full meter = 3600. Decreases 1 per frame underwater, increases 32 per frame above surface. Starts decreasing 2 seconds after going underwater. Decreases 225 (1/16th) per Luigi spin. After reaching 0 air, take 1 damage after 4 seconds, and then additional damage every 3 seconds.
