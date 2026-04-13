#csapp 
## Strategy Overview
- **Free Block Tracking:** Explicit Free List (For now)
- **Placement Policy:** First Fit
- **Splitting and Coalescing**: Split if the remains is larger than minimum; Coalescing on Freeing using boundary tags

### Boundary tags
- Both Header & Footer for Free blocks, only Header for Allocated blocks.
- Tag = Block Size (Aligned by 8) + prev_allocated? + a/f
- Prologue Block & Epilogue Block (Marked as Allocated)

## Data Structure
### Block
**Free**:
- Header Tag
- Prev Pointer
- Next Pointer
- Padding (Optional)
- Footer Tag
**Minimum:** 16 bytes

**Allocated**:
- Header Tag
- Payload (At least 4 bytes)
- Padding (Optional)
**Minimum:** 8 bytes

## Explicit Free List