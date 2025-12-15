# Release v0.1.1

Date: 2025-12-15

Highlights:
- Fixed and hardened `StableHashSplit`:
  - Support `id_column=None` to use DataFrame index
  - Normalized CRC32 hashing into [0,1) for correct thresholds
  - Input length validation when `y` provided
  - Vectorized hash mask computation for performance
- Updated packaging metadata and documentation

See `CHANGELOG.md` for full history.
