#csapp 
## Strategy Overview
- **Free Block Tracking:** Segregated Free List
- **Placement Policy:** First Fit
- **Splitting and Coalescing**: Split if the remains is larger than minimum; Coalescing on Freeing using boundary tags

### Boundary tags
- Both Header & Footer for Free blocks, only Header for Allocated blocks.
- Tag = Block Size (Aligned by 8) + prev_allocated? + a/f

## Implementation
### Explicit Free List
