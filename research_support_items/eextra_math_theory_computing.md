# Learning Backlog: Computing, Math, Theory

Broad list of skills and study items supporting current goals, plus a protocol for coding and software side items.

## Computing

### General programming plan

Potentially paired with the Erdos Institute program. Later items can start while the ML/DS work is in progress.

1. Finish the OOP course.
2. Finish all AOC 2025 problems and host the solutions on a website.
3. ML and deep learning: Andrew Ng's deep learning course, math for ML (bookmarks), other MIT ML courses. Then one or two advanced SQL courses.
4. Build a project once the above is done.
5. Quickly move through scientific programming courses as a refresher.
6. Work through Jon's 641 problems alongside the 512 problems.
7. Algorithms and data structures, possibly in C (University of Colorado Boulder has a course).
8. Parallel programming with C. Start parts of this early.
9. Databases: YouTube lecture series plus notes in personal ed.
10. Linux and the command line. Start anytime and make steady progress.

### Linux and the command line

- [OverTheWire](https://overthewire.org/wargames/) wargames (recommended by Mohan)
- [*The Linux Command Line*](https://linuxcommand.org/index.php)
- Cynthia's Linux bootcamp file

### GPU computing

- Rework the performance progression in [How to Optimize a CUDA Matmul Kernel](https://siboehm.com/articles/22/CUDA-MMM) (siboehm)
- [GPU MODE lectures](https://www.youtube.com/@GPUMODE/videos)
- [NVIDIA Accelerated Computing Hub](https://github.com/NVIDIA/accelerated-computing-hub/blob/main/README.md): many hands-on Colab notebooks on core concepts

### C/C++

- [isocpp.org Get Started](https://isocpp.org/get-started): standard reference for learning and keeping up with C++
- Bro Code C/C++ courses
- GeeksforGeeks C projects: take an existing project idea and add personal modifications

### Initial backlog (from Overleaf)

- Two OOP projects
- Begin working through *The Linux Command Line*

## Math

- **Statistical inference book:** tools for building intuition for current research.
- **Proofs book.**
- **Group and representation theory:** less pressing, but grounds many physics and math concepts.
- **Pencil-and-paper exercises in machine learning** (paper in Zotero): not research-central, but many ideas carry over and it builds statistical thinking.

### Past items

- Spherical Bessel functions
- Complex analysis; complex numbers and statistics with complex-valued variables

## Theory

- **Radiative processes:** keep the rough structure from the end of the course and work through the remainder of Rybicki & Lightman. Planned to resume in October unless other priorities or unfinished work take precedence.
- **Full cosmology review:** notes in bookmarks, Baumann, Tong. Start with the CMB, then return to the basics.

## Research support items

Extra coding/Math/Theory or software items (***DIRECTLY***) tied to research. **Pull one out only outside work hours, or when the research genuinely requires it.** Each item must answer all four fields before it gets time.

| Field | Question |
|---|---|
| **What** | What is the item? |
| **Why** | What concrete problem does it solve? |
| **Resource** | What will I learn or work from? |
| **Capacity** | What does proficiency or completion mean? State a checkable end point. |

### Template

#### <Item name>
- **What:**
- **Why:**
- **Resource:**
- **Capacity:**
- **Status:** not started / in progress / done


### Items
 
#### CUDA matmul optimization progression
- **What:** Rework siboehm's sequence of SGEMM kernels from naive to 2D block tiling with vectorized loads.
- **Why:** The source level (`Sig_prime`) lags the diffuse level at ~4–5× speedup, and the n_eig=20, n_src=20 corner risks register spills. Shared-memory tiling, register blocking and occupancy trade-offs are the tools for both.
- **Resource:** [How to Optimize a CUDA Matmul Kernel](https://siboehm.com/articles/22/CUDA-MMM)
- **Capacity:** Each kernel reimplemented and benchmarked on the A40 against cuBLAS SGEMM, with an Nsight Compute profile and a one-line bottleneck explanation per step. The final kernel reaches at least 80% of cuBLAS.
- **Status:** not started
#### Reductions, atomics and profiling
- **What:** Reduction patterns (warp shuffle, block-level, hierarchical), atomic contention, and Nsight Compute stall analysis.
- **Why:** `cap_reduce` shows 58.5% MIO scoreboard stalls from atomic contention.
- **Resource:** [GPU MODE lectures](https://www.youtube.com/@GPUMODE/videos) on reductions and profiling; [NVIDIA Accelerated Computing Hub](https://github.com/NVIDIA/accelerated-computing-hub/blob/main/README.md) notebooks
- **Capacity:** `cap_reduce` rewritten so MIO scoreboard stalls drop measurably in Nsight Compute, with the source-level speedup re-benchmarked. Register usage at the n_eig=20, n_src=20 corner verified with `nvcc -Xptxas -v`.
- **Status:** not started
#### Modern C++ for kernel and host code
- **What:** Templates, compile-time parameters, RAII and the C++ features used in CUDA host and device code.
- **Why:** gpu_corrcal relies on template flags (`COMPUTE_DET`, `IS_INV`). The upcoming `apply_gains` and `nll_and_grad` kernels need the same patterns written cleanly.
- **Resource:** [isocpp.org Get Started](https://isocpp.org/get-started); Bro Code C/C++ courses as a quick refresher
- **Capacity:** Every C++ construct in the gpu_corrcal source can be explained without lookup, and `apply_gains` is written with template specialization and no duplicated kernel code.
- **Status:** not started
#### Parallel programming in C (CPU)
- **What:** OpenMP-level multithreading and SIMD in C.
- **Why:** GPU speedup claims in the paper are only credible against a fair multithreaded CPU baseline, not single-threaded NumPy.
- **Resource:** To choose (OpenMP tutorial or course)
- **Capacity:** An OpenMP CPU implementation of the Woodbury inverse matvec, benchmarked across thread counts and added as a series in the benchmark registry.
- **Status:** not started
#### Linux and the command line
- **What:** Shell fluency, scripting and cluster workflow.
- **Why:** Cluster work depends on it, and the stale `.so` binary bug was a build and deploy failure.
- **Resource:** [OverTheWire](https://overthewire.org/wargames/) Bandit (recommended by Mohan); [*The Linux Command Line*](https://linuxcommand.org/index.php); Cynthia's Linux bootcamp file
- **Capacity:** All Bandit levels complete. A cluster build script rebuilds and verifies the `.so` (timestamp or hash check) before every benchmark run.
- **Status:** not started
#### Jon's PHYS 512 and 641 problems
- **What:** Sievers' course problem sets, worked in parallel.
- **Why:** They exercise numerical and statistical methods in the form Sievers uses, which is the form CorrCal is written in.
- **Resource:** The 512 and 641 problem sets
- **Capacity:** Every problem solved in code in one repo, starting with those on linear solvers and likelihood fitting.
- **Status:** not started
#### Statistical inference
- **What:** Estimation theory: bias, variance, Fisher information, Cramér-Rao bound, likelihood-based inference.
- **Why:** Sub-study 1 of the quasi-redundant calibration paper compares recovered gain variance to the Cramér-Rao bound.
- **Resource:** Statistical inference book (to choose); Kay, *Estimation Theory*, ch. 3 and 15
- **Capacity:** Fisher information for the gains derived under the CorrCal covariance, and the CRB computed for a small array, matching the empirical gain variance from simulated realizations.
- **Status:** not started
#### Pencil-and-paper exercises in machine learning
- **What:** Exercise sets on optimization, latent-variable models, state-space models and sampling.
- **Why:** These are the methods behind the bandpass smoothness prior (Gaussian process) and the gain-stability Kalman filter direction. They also build statistical reasoning.
- **Resource:** Pen-and-paper exercises paper (Zotero)
- **Capacity:** The optimization and hidden Markov / state-space chapters complete, with written solutions.
- **Status:** not started
#### Complex-valued statistics and calculus
- **What:** Circular complex Gaussian distributions and Wirtinger calculus.
- **Why:** Visibilities and gains are complex, and the gain gradient of dᴴC⁻¹d depends on getting complex derivatives right.
- **Resource:** Complex analysis text (to choose); Kay ch. 15
- **Capacity:** The gradient of dᴴC⁻¹d with respect to complex gains derived by hand with Wirtinger derivatives, matching the implemented GPU gradient numerically.
- **Status:** not started
#### Spherical Bessel functions
- **What:** The plane-wave expansion in spherical harmonics and spherical Bessel functions.
- **Why:** The full-sky diffuse visibility covariance uses this expansion. The current model uses the flat-sky approximation (`_compute_diffuse_matrix_from_flat_sky`).
- **Resource:** To choose (e.g. Arfken, mathematical methods text)
- **Capacity:** The full-sky diffuse visibility covariance derived, then compared numerically to the flat-sky result to show where the approximation breaks down with baseline length.
- **Status:** not started
#### Radiative processes
- **What:** The remainder of Rybicki & Lightman, keeping the rough structure from the end of the course.
- **Why:** Synchrotron foregrounds are spectrally smooth. Bandpass calibration has to preserve that smoothness to keep foreground power out of 21 cm modes.
- **Resource:** Rybicki & Lightman
- **Capacity:** Radiative transfer and synchrotron chapters complete, with the power-law synchrotron spectrum derived from the electron energy distribution.
- **Status:** not started (planned for October)
#### Cosmology review
- **What:** A full cosmology review, CMB first, then the basics.
- **Why:** CHORD's science case is 21 cm intensity mapping. Calibration requirements are set by how gain errors propagate into the 21 cm power spectrum.
- **Resource:** Notes in bookmarks; Baumann; Tong
- **Capacity:** The 21 cm brightness temperature and its power spectrum derived, with an explanation of how calibration errors leak foreground power into 21 cm modes.
- **Status:** not started
 