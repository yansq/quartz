
##  branching factor

If the branching factor is b, then the _actual_ number of children m for a given internal node is constrained such that `b/2 <= m <= b`.

## insert

- Step 1 find the correct laaf L
- Step 2 put data entry onto L
    - Step 2-1 if L has enough space, done
    - Step 2-2 else, must split L (into L and a new node L2)
        - redistribute entries evenly, copy up ( or push) the middle key
        - insert index entry pointing to L2 into the parent of L

Step 2 can happen recursively, when running 2-2, and the parent node is full.
In step 2-2,  use copying in leaf layers, and use pushing in index layers.

