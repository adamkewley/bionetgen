# Bionetgen

Bionetgen is a brief C++ tool to create fiber networks that follow the fiber length, valency and cosine distribution of actual collagen gels from confocal microscope images. Our algorithm closely follows the approach of [1] using the stochastic optimization method of simulated annealing.

[1] S.B. Lindstrom, D.A. Vader, A. Kulachenko, D.A. Weitz, Biopolymer network geometries: Characterization,
regeneration, and elastic properties. Phys. Rev. E - Stat. Nonlinear, Soft Matter Phys. 82(5), 2 (2010).


## Building

    $ mkdir build
    $ cd build
    $ cmake ..
    $ make
    $ cd ..

## Building on Windows

These build instructions use the MinGW64 part of [MSYS2](https://www.msys2.org/), which provides the necessary tools (i.e. `cmake`, `make`, `g++`, and `boost`). It generates a standalone executable that only depends on Windows' core libraries.

The reason this guide uses MinGW64 under MSYS2, despite MSYS2 generally being considered to be more modern, is because MinGW can be used to create standalone executables that ultimately do not depend on either MSYS2 or MinGW64 to run them.

- Install [MSYS2](https://www.msys2.org/)
- Clone this repository onto your computer
- Boot into an `MSYS2_MinGW64` terminal
- Go through these commands:

```bash
# install `cmake`, `make`, `gcc`, `g++`, and `boost`
pacman -S mingw-w64-x86_64-cmake make mingw-w64-x86_64-gcc mingw-w64-x86_64-boost

# change to the cloned repository
cd /C/Users/user/Desktop/bionetgen

# build with static linking
mkdir build
cd build
cmake .. -DCMAKE_EXE_LINKER_FLAGS="-static"
cmake --build .

# check voronoi.exe exists, and that it only depends on Windows libraries
#
# (i.e. every library it lists should be in /C/Windows)
ldd voronoi.exe
```

## Usage
    $ ./build/voronoi config.json

## Configuration

Use the config.json file to set parameters for the algorithm.

The *main* operation mode can be selected by changing the values of *generate* and *simulate* in the config file. If only generate is true, a voronoi geometry will be generated and saved as the output. If simulate is also true, simulated annealing will additionally be performed before the geometry is saved to the output files. If only simulate is true, a previously saved geometry will be read from the input files, simulated annealing will be perfomed and the result will be saved to the output files.

### Publications

Please cite the following paper for acknowledging this software contribution:

Eichinger JF, Grill MJ, Davoodi Kermani I, Aydin RC, Wall WA, Humphrey JD, Cyron CJ, A computational framework for modeling cell-matrix interactions in soft biological tissues. *Biomechanics and Modeling in Mechanobiology*. 2021. doi.org/10.1007/s10237-021-01480-2

### Parameters

*seed*: The seed for the random number generator (can be any integer number)

*particles*: The number of Voronoi particles (can be any number greater 5)

*input-prefix*: The path prefix of the input files if simulated annealing is done on a previously generated geometry (relative to the working directory, optional)

*output-prefix*: The path prefix for the output files (relative to the working directory, not the )

*generate*: If a voronoi geometry should be generated (can be true or false)

*simulate*: If simulated annealing should be performed on a voronoi geometry (can be true or false)

*box-size*: The size of the box containing the voronoi geometry to be generated (must be an array of 3 positive values)

*box-origin*: The origin of the box (must be an array of 3 values)

### *simulated-annealing*: Options for simulated annealing (if needed)

*mode*: Simulated annealing mode (can be 1, 2 or "both")

*max-iter*: Maximum number of iterations  (can be any positive integer)

*max-subiter*: Maximum number of sub-iterations (can be any positive integer)

*weight-line*: Line weight (can be any positive value)

*weight-cosine*: Cosine weight (can be any positive value)

*tolerance*: Tolerance (can be any positive value)

*temperature-initial*: Initial temperature (can be any positive value)

*temperature-decay-rate*: Temperature decay rate  (can be any positive value)

*max-movement-frac*: Maximum movement fraction (can be any value between 0 and 1)

*screen-output-every*: After how many iterations screen output is produced (can be any positive integer)

*num-bins-length*: Number of bins per length (can be any positive integer)

*num-bins-cosine*: Number of bins per cosine (can be any positive integer)

### License

*bionetgen* is published under the BSD 3-Clause License.

### Acknowledgements

This project uses the library voro++ by Chris Rycroft from University of California, through Lawrence Berkeley National Laboratory, for the generation of the voronoi geometry, which can be downloaded from http://math.lbl.gov/voro%2B%2B/.

