Nessun problema. Ecco il README completo in inglese, con le tabelle dei risultati integrate.

---

# Traveling Salesperson Problem (TSP)

I used a solution based on an hybrid of a Genetic Algorithm and Local Search (Memetic Algorithm). 
The objective is to find the cyclical path that visits every city exactly once with the lowest possible **cost (distance)**.


## Methodology

The implementation is enhanced with several techniques:

* **Specialized Memetic Algorithms:** Two distinct algorithms are used:
    * `memetic_algorithm_2opt`: For symmetric problems (`g_`).
    * `memetic_algorithm_ATSP`: For asymmetric problems (`r1_`, `r2_`).
* **Standard Genetic Operators:**
    * **Selection:** Tournament Selection.
    * **Crossover:** Order Crossover (OX1).
    * **Mutation:** A hybrid strategy that randomly applies either a *Swap* or *Insert* mutation.
* **O(1) Local Search (Hill Climbing):** The core of this approach. Specific moves with constant-time "delta" calculation (O(1)) are used to speed up the search:
    * **2-Opt (for `g_`):** The `apply_local_search_2opt_fast` function implements the 2-Opt move, which is valid only for symmetric problems.
    * **Insert (for `r1_`, `r2_`):** The `apply_local_search_insert_ATSP` function implements the *insert* move, which is correct and efficient for asymmetric problems.
* **JIT Optimization:** Critical calculation functions (solution evaluation and local search) are accelerated using Numba's `@jit(nopython=True)` decorator.

## Results

Parameters were tuned for each category to balance solution quality and execution time.

### Category G (Symmetric)

* **Algorithm:** `memetic_algorithm_2opt`
* **Parameters (MA-2opt-FAST):**
    * `population_size`: 200
    * `generations`: 400
    * `elite_size`: 15
    * `mutation_rate`: 0.1
    * `local_search_rate`: 0.25

| Problem | MA Cost | MA Time (s) |
| :--- | :--- | :--- |
| `problem_g_10.npy` | 1497.663648 | 0.746032 |
| `problem_g_20.npy` | 1755.514677 | 1.226844 |
| `problem_g_50.npy` | 2629.986686 | 2.372823 |
| `problem_g_100.npy` | 3957.492539 | 5.665192 |
| `problem_g_200.npy` | 5422.414308 | 12.987909 |
| `problem_g_500.npy` | 8320.242597	 | 102.723349 |
| `problem_g_1000.npy`| 11768.389705 | 674.883598 |


### Category R1 (Asymmetric, `r1_`)

* **Algorithm:** `memetic_algorithm_ATSP`
* **Parameters (MA-Insert-FAST):**
    * `population_size`: 200
    * `generations`: 450
    * `elite_rate`: 0.11
    * `mutation_rate`: 0.2
    * `local_search_rate`: 0.20

| Problem | MA Cost | MA Time (s) |
| :--- | :--- | :--- |
| `problem_r1_10.npy` | 184.273441 | 1.226613 |
| `problem_r1_20.npy` | 337.294872 | 1.429031 |
| `problem_r1_50.npy` | 558.039596 | 3.019262 |
| `problem_r1_100.npy` | 702.797189 | 6.116293 |
| `problem_r1_200.npy` | 1012.731107 | 13.988372 |
| `problem_r1_500.npy` | 1809.136027 | 60.815880 |
| `problem_r1_1000.npy`| 3337.506671 | 306.584696 |


### Category R2 (Asymmetric, `r2_`)

* **Algorithm:** `memetic_algorithm_ATSP`
* **Parameters (MA-Insert-FAST-JIT):**
    * `population_size`: 200
    * `generations`: 450
    * `elite_rate`: 0.15
    * `mutation_rate`: 0.2
    * `local_search_rate`: 0.20

| Problem | MA Cost | MA Time (s) |
| :--- | :--- | :--- |
| `problem_r2_10.npy` | -411.701716 | 0.913319 |
| `problem_r2_20.npy` | -861.667206 | 1.383095 |
| `problem_r2_50.npy` | -2264.853056 | 2.935205 |
| `problem_r2_100.npy` | -4695.534714 | 6.071149 |
| `problem_r2_200.npy` | -9457.479187 | 13.380420 |
| `problem_r2_500.npy` | -23769.842101 | 50.223049 |
| `problem_r2_1000.npy`| -47807.420631 | 205.554488 |
