# Reference pointer based values

These values aren't identified as being part of a particular memory "block", but they can all be found based on the reference pointer.


## Items

Item | Offset | Bytes | Format | Notes
--- | --- | --- | --- | ---
Position | 0x18DC | 12 | Float 3D vector | Editable
Gravity down-vector | 0x1B10 | 12 | Float 3D vector
Life meter | 0x1C50 | 4 | Integer
General state | 0x3DC4 | 4 | Binary
Position, 1 frame late | 0x3EEC | 12 | Float 3D vector | Not editable. This value should be in sync with what's shown on the game screen
Base velocity | 0x3F64 | 12 | Float 3D vector | Not all kinds of movement are covered. For example, launch stars and riding moving platforms aren't accounted for
Tilt up-vector | 0x3FAC | 12 | Float 3D vector
Gravity up-vector | 0x6A3C | 12 | Float 3D vector