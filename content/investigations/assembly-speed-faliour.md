---

title: "Why Adding Assembly Didn't Make My Quantum Simulator Faster"
description: "An investigation into the performance impact of introducing hand-written assembly into a C++ quantum circuit simulator."
case_id: "001"
status: "RESOLVED"
date: "2026-06-12"
category: "High Performance Computing"
tags: ["Performance", "Assembly", "C++", "Eigen", "Investigation"]
---

# Overview

While developing a quantum circuit simulator in C++, I assumed that replacing part of the computation with hand-written assembly would improve performance. Assembly is often regarded as the fastest way to execute code because it provides direct control over the processor and avoids compiler abstractions.

To test this assumption, I implemented portions of the simulator using assembly and compared the results against the existing implementation built with C++ and the Eigen linear algebra library.

The result was unexpected: the assembly implementation did not provide a measurable performance improvement and, in some cases, performed worse than the original C++ version.

This investigation documents why.

# Initial Hypothesis

My original assumption was straightforward:

> Hand-written assembly should execute faster than equivalent C++ code.

Since quantum circuit simulation involves large numbers of matrix operations, reducing execution time could significantly improve simulator performance.

I expected assembly to provide:

* Reduced instruction overhead
* Better control over register usage
* More efficient mathematical operations

# The Existing Implementation

The simulator was written in C++ and used the Eigen library for matrix and vector operations.

Eigen is a highly optimized linear algebra library that makes extensive use of:

* Expression templates
* SIMD vectorization
* Compiler optimizations
* Cache-aware algorithms

At the time, I underestimated how much optimization Eigen was already performing on my behalf.

# Experiment

To evaluate the potential benefit of assembly, I implemented a portion of the computational path using hand-written assembly.

The goal was to reduce the cost of frequently executed mathematical operations.

After integrating the assembly code, I measured execution times and compared them against the original implementation.

# Results

The assembly implementation failed to produce a meaningful performance improvement.

Although individual assembly routines executed efficiently, the overall execution time of the application remained largely unchanged.

In some scenarios, the assembly version performed slightly worse.

# Root Cause Analysis

After examining the implementation more closely, I realized that the assembly instructions themselves were not the primary bottleneck.

The real cost came from the surrounding integration layer.

The application needed to:

1. Prepare data in C++.
2. Transfer data into the assembly routine.
3. Execute the assembly code.
4. Transfer results back into C++ structures.

The overhead associated with these transitions reduced or completely eliminated any gains achieved inside the assembly routine.

At the same time, Eigen was already generating highly optimized machine code through compiler-assisted vectorization and mathematical optimizations.

As a result, the assembly implementation was competing against code that had already been heavily optimized.

# Lessons Learned

This investigation changed how I think about performance optimization.

The most important lesson was:

> Faster code in isolation does not necessarily produce a faster system.

Performance must be evaluated at the system level rather than at the level of individual functions.

I also learned that modern compilers and specialized libraries can often outperform manually optimized implementations, particularly when they have access to architecture-specific optimizations and years of engineering effort.

# Conclusion

Adding assembly to the quantum simulator did not improve performance because the computational gains were offset by integration overhead, while the existing Eigen-based implementation was already highly optimized.

The investigation reinforced an important engineering principle:

> Optimization should be guided by measurement rather than assumptions.

Before introducing additional complexity, it is essential to identify the true bottlenecks and verify that a proposed optimization addresses them.
