
# Cache Coherence and NUMA Performance Lab

## Overview

This project explores cache coherence protocols and memory-access performance using Python, C, and OpenMP in Ubuntu running on VMware.

## Objectives

- Simulate MSI, MESI, and MOESI cache coherence protocols.
- Compare modeled bus transactions, invalidations, writebacks, and cache-to-cache transfers.
- Demonstrate false sharing using adjacent and padded counters.
- Measure OpenMP performance with different thread counts.
- Examine memory placement and NUMA topology.

## Technologies Used

- Ubuntu Linux
- C and GCC
- OpenMP
- Python 3
- numactl
- hwloc

## Project Files

| File | Description |
|---|---|
| `coherence_sim.py` | Simulates MSI, MESI, and MOESI cache states and transaction counts |
| `false_sharing.c` | Compares adjacent and padded thread-local counters |
| `numa_sweep.c` | Measures repeated memory-sweep execution time |
| `compare_numa.c` | Combines false-sharing and memory-sweep benchmarks |
| `screenshots/` | Terminal screenshots and experimental results |

## Setup

Install the required tools on Ubuntu:

```bash
sudo apt update
sudo apt install build-essential numactl hwloc python3 -y
```

## Run the Cache Coherence Simulation

```bash
python3 coherence_sim.py 1000
```

## Run the False-Sharing Benchmark

```bash
gcc -O2 false_sharing.c -fopenmp -o false_sharing

OMP_NUM_THREADS=2 ./false_sharing adjacent
OMP_NUM_THREADS=2 ./false_sharing padded
```

## Run the NUMA Memory Sweep

```bash
gcc -O2 numa_sweep.c -fopenmp -o numa_sweep

OMP_NUM_THREADS=1 ./numa_sweep
OMP_NUM_THREADS=2 ./numa_sweep
```

## Run the Combined Benchmark

```bash
gcc -O2 compare_numa.c -fopenmp -o compare_numa
./compare_numa
```

## Hardware Environment

The experiment was run inside an Ubuntu VMware virtual machine. The observed environment exposed four logical CPUs and one NUMA node.

## Results and Screenshots

### 1. Ubuntu Environment and NUMA Hardware

![Ubuntu environment and NUMA hardware](screenshots/01-ubuntu-environment-and-numa-hardware-setup.png)

### 2. MSI, MESI, and MOESI Simulation

![Cache coherence simulation](screenshots/02-msi-mesi-moesi-cache-coherence-simulation.png)

### 3. False-Sharing Benchmark

![False sharing benchmark](screenshots/03-false-sharing-adjacent-vs-padded-counters.png)

### 4. NUMA Memory Sweep

![NUMA memory sweep](screenshots/04-numa-memory-sweep-single-vs-multithread.png)

### 5. Combined Benchmark Results

![Combined benchmark results](screenshots/05-combined-false-sharing-and-memory-sweep-results.png)

### 6. NUMA Node-Bound Benchmark

![NUMA node-bound benchmark](screenshots/06-numa-node-bound-benchmark-results.png)

## Observations

- The simulator illustrates how protocol states influence modeled coherence traffic.
- Padding can reduce false sharing by separating counters onto different cache lines.
- OpenMP execution time varies with thread count and system conditions.
- The available virtual machine exposed only one NUMA node, so a true remote-versus-local NUMA comparison was not performed.

## Conclusion

The experiments demonstrate the relationship between cache coherence, false sharing, parallel execution, and memory placement. Performance results depend on the implementation, compiler, CPU, virtual machine configuration, and system load.
