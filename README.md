# Cheney's Stop-and-Copy Garbage Collection

## Project Overview
This repository implements Cheney's algorithm, a Breadth-First Search (BFS) based stop-and-copy garbage collection method designed for memory management. The code provides a formally structured, highly typed Ada implementation utilizing two Semi-Spaces ("From-Space" and "To-Space") to safely relocate reachable memory nodes and implicitly compact the heap. 

## Features
* **Dual Semi-Space Heap Management:** Clean architectural separation of active and inactive memory blocks.
* **Variant - Graph Topologies:** Out-of-the-box support for arbitrary directed graph topologies.
* **Variant - Cyclic Defense:** Leaves forwarding addresses in old nodes to prevent infinite recursion on cyclical references `(A -> B -> A)`.
* **Variant - Shared Nodes (Diamonds):** Correctly relocates shared nodes `(Root1 -> A, Root2 -> A)` only once.
* **Algorithmic Correctness:** Preserves the core Cheney invariant: Implicitly organizes moved memory strictly according to BFS order.

## Testing (Verification & Validation)
This project enforces rigorous Validation and Verification (V&V) principles suited for critical systems. The test suite operates on a **pessimistic philosophy**: it assumes the codebase is fundamentally broken, missing bounds checks, or susceptible to pointer corruption. A test only registers a `PASS` when it definitively disproves this pessimistic assumption.

**What the tests verify:**
1. **Functional Correctness (T5-T8, T11):** Ensures Cheney's core BFS invariants are met. Determines if the scanner correctly updates references rather than breaking the heap topology.
2. **Robustness & Edge Cases (T9, T10, T13):** Proves the code withstands self-references, cyclic dependencies, and multi-root shared references without duplicating memory or hitting infinite recursion.
3. **Error Handling (T4):** Validates that constraint ceilings (Out-Of-Memory limits) are enforced immediately, preventing buffer overruns.

**Why these tests matter:**
In systems programming, garbage collection failures result in silent corruption, duplicate instances, or system lockups. By formulating the test suite to target adversarial edge-cases (cycles, out-of-bounds bounds), we mathematically demonstrate reliability. 

## Usage
### Requirements
* GNAT Ada Compiler (`gnatmake`)
* `make` utility

### Compilation
To build both the main executable and the test suite:
```bash
make all
