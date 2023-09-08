<div align="center">

# High-Performance Linpack (HPL) ROCK

An Ubuntu 22.04 LTS-based OCI image for running HPL, a portable implementation of the High-Performance Linpack Benchmark for Distributed-Memory Computers. HPL is used to 
benchmark the [Top 500 supercomputers](https://www.top500.org).


[![CI](https://github.com/canonical/hpl-rock/actions/workflows/ci.yaml/badge.svg)](https://github.com/canonical/hpl-rock/actions/workflows/ci.yaml/badge.svg)
[![Release](https://github.com/canonical/hpl-rock/actions/workflows/release.yaml/badge.svg)](https://github.com/canonical/hpl-rock/actions/workflows/release.yaml/badge.svg)
[![Matrix](https://img.shields.io/matrix/ubuntu-hpc%3Amatrix.org?logo=matrix&label=ubuntu-hpc)](https://matrix.to/#/#ubuntu-hpc:matrix.org)

</div>

## Features

The High-Performance Linpack (HPL) OCI image provides a containerised `xhpl` binary that 
can be used to run the Linpack benchmark on your distributed-memory system. OpenBLAS, ATLAS,
mpich, and libfabric are installed inside this image to support the `xhpl` binary.

## Usage

This short usage how-to section assumes that you have [Sarus](https://sarus.readthedocs.io/en/stable/quickstart/quickstart.html) 
installed on your distributed-memory system, however, this image should work with any 
OCI-compliant container runtime.

### With a single node

#### Copy the example _HPL.dat_ file to your local system:

<details>

<summary><code>HPL.dat</code></summary>


```text
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
124          Ns
1            # of NBs
64           NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
1            Ps
1            Qs
16.0         threshold
1            # of panel fact
2            PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
4            NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
1            RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
1            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
1            DEPTHs (>=0)
2            SWAP (0=bin-exch,1=long,2=mix)
64           swapping threshold
0            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

</details>

#### Execute `xhpl` using Sarus

```shell
sarus pull ghcr.io/canonical/hpl:2.3
sarus run \
  --entrypoint "" \
  --mount type=bind,source=$(pwd)/HPL.dat,destination=/HPL.dat \
  ghcr.io/canonical/hpl:2.3 xhpl
```

### With multiple nodes

#### Copy the example _HPL.dat_ file to your local system

<details>

<summary><code>HPL.dat</code></summary>


```text
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
24000        Ns
1            # of NBs
128          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
8            Ps
8            Qs
16.0         threshold
1            # of panel fact
2            PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
4            NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
1            RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
1            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
1            DEPTHs (>=0)
2            SWAP (0=bin-exch,1=long,2=mix)
64           swapping threshold
0            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

</details>

#### Execute `xhpl` using Sarus through `mpirun`

```shell
sarus pull ghcr.io/canonical/hpl:2.3
mpirun -np 64 sarus run \
  --entrypoint "" \
  --mount type=bind,source=$(pwd)/HPL.dat,destination=/HPL.dat \
  ghcr.io/canonical/hpl:2.3 xhpl
```

> __Note__: The provided _HPL.dat_ files may not be optimal for your distributed-memory
> system. You may want to modify your _HPL.dat_ file to identify the optimal
> parameters for your system. See the [Tuning](https://netlib.org/benchmark/hpl/tuning.html)
> section of the HPL documentation for more information on modifying the _HPL.dat_ file.

## Project & Community

The HPL OCI image is a project of the [Ubuntu HPC](https://discourse.ubuntu.com/t/high-performance-computing-team/35988) 
community. It is an open source project that is welcome to community involvement, contributions, suggestions, fixes, and 
constructive feedback. Interested in being involved with the development of hpl-rock? Check out these links below:

* [Join our online chat](https://matrix.to/#/#ubuntu-hpc:matrix.org)
* [Contributing guidelines](./CONTRIBUTING.md)
* [Code of conduct](https://ubuntu.com/community/ethos/code-of-conduct)
* [File a bug report](https://github.com/canonical/hpl-rock/issues)
* [Rockcraft docs](https://canonical-rockcraft.readthedocs-hosted.com/en/latest/)

## License

This HPL OCI image is free software, distributed under the Apache Software License, 
version 2.0. See the [LICENSE](./LICENSE) file for more information.
