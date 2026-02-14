# Paradox 4.0 — Apple Silicon Port

A finite model finder for first-order logic (TPTP format), by Koen Claessen and Niklas Sorensson.

See [the official page](http://vlsicad.eecs.umich.edu/BK/Slots/cache/www.cs.chalmers.se/~koen/paradox/)
for a description, papers, and original authors.

## History

This is a fork of [c-cube/paradox](https://github.com/c-cube/paradox), which
recovered the last available Paradox distribution (circa 2010) and patched it
for GHC 7.10 and C++11. This fork adds further patches to build on
**Apple Silicon (arm64) Macs** with modern toolchains.

## Changes from upstream

- **Relaxed `base` version bound** — removed the `< 4.9` upper bound so the
  package builds with current GHC (9.x)
- **Added `MonadFail` instance** for `Parser` (`src/Parsek.hs`) — required
  since GHC 8.6 (the `MonadFail` proposal)
- **Fixed default argument on friend template declaration**
  (`src/minisat/current-base/SolverTypes.h`) — Clang rejects default arguments
  on friend declarations; added a separate overload instead
- **Renamed output binary** to `paradox.bin` — avoids a conflict with the
  `Paradox/` source directory on case-insensitive filesystems (macOS)
- **Regenerated minisat dependency file**

## Build

### Prerequisites

- [GHC](https://www.haskell.org/ghcup/) (tested with 9.x)
- A C++ compiler with C++11 support (Apple Clang works)
- GMP (for GHC; `brew install gmp` on macOS)

### Using the Makefile directly

```bash
cd src && make
```

The binary will be at `src/paradox.bin`.

### Using Stack

```bash
stack setup
stack init      # if stack.yaml needs regenerating
stack exec make
```

Note: the `stack.yaml` shipped here pins `lts-6.9` (GHC 7.10). You may need
to update the resolver for a modern GHC.

## Usage

```bash
./src/paradox.bin --help
./src/paradox.bin problem.tptp
```

## License

GPL-2 (see [LICENSE](LICENSE)).
