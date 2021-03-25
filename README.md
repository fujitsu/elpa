# [Eigenvalue SoLvers for Petaflop-Applications (ELPA)](http://elpa.mpcdf.mpg.de)

This is a patch of the [ELPA 2016.11.001.pre](https://gitlab.mpcdf.mpg.de/elpa/elpa/-/tree/ELPA_2016.11.001.pre) for Fujitsu Development Studio(FUJITSU Software Compiler Package) with the Armv8-A 64-bit architecture (AArch64).
See the [ELPA](https://gitlab.mpcdf.mpg.de/elpa/elpa/-/tree/ELPA_2016.11.001.pre) home page for more information.

**WARNING**:
ELPA requires an MPI implementation with _MPI_THREAD_MULTIPLE_ support. On the other hand, the FUJITSU MPI only supports an MPI implementation with _MPI_THREAD_SERIALIZED_.  Not all functions in ELPA have required the _MPI_THREAD_MULTIPLE_ support. The following functions, which require the _MPI_THREAD_MULTIPLE_ support, are **not** available in the Fujitsu Development Studio(FUJITSU Software Compiler Package):
```
elpa_solve_evp_complex_2stage
elpa_solve_evp_real_2stage
solve_evp_complex_2stage	
solve_evp_real_2stage	
```


# Prerequisites

* Fujitsu Development Studio or FUJITSU Software Compiler Package

# Installation

## Download the source code
* Clone this repository.

```
$ git clone https://github.com/fujitsu/elpa.git
```

* Download ELPA 2016.11.001.pre.

```
$ cd elpa
$ wget https://elpa.mpcdf.mpg.de/html/Releases/2016.11.001.pre/elpa-2016.11.001.pre.tar.gz
```

* Apply the patch

```
$ tar -zxvf elpa-2016.11.001.pre.tar.gz
$ patch -p0 < elpa-2016.11.001.pre_a64fx.patch
```

## Building ELPA

* Native compilation using Fujitsu compiler (AArch64 target).

```
$ cd elpa-2016.11.001.pre
$ ./configure                           \
    --prefix=${INSTALL_DIR}             \
    --build=aarch64-linux-gnu           \
    --enable-openmp                     \
    --enable-shared                     \
    --enable-static                     \
    CC=mpifcc                           \
    FC=mpifrt                           \
    FCFLAGS="-KPIC -Kocl -Kregion_extension -Knolargepage -Kfast -Kopenmp -Koptmsg=2 -Nlst=t"   \
    CFLAGS="-KPIC -Kocl -Kregion_extension -Knolargepage -Kfast -Kopenmp -Koptmsg=2 -Nlst=t"   \
    LDFLAGS="-SSL2BLAMP"
$ make -j30

```


## Run tests

```
$ make check
```
**WARNING**:
If you have an older version of the Fujitsu Development Studio(FUJITSU Software Compiler Package), you may have a FAIL result.
This is a FAIL with a warning message, not a FAIL that affects the results.
If "test-suite.log", which is generated after the test, contains "(exit status: 4)", this FAIL has occurred.

## Install ELPA

```
$ make install
```

# Usage

## How to compile using test program in "elpa-2016.11.001.pre/test_project/src/test_real.F90"
```
$ mpifrt -L${INSTALL_DIR}/lib -I${INSTALL_DIR}/include/elpa_openmp-2016.11.001.pre/modules -lelpa_openmp -SSL2BLAMP -SCALAPACK test_real.F90
```


