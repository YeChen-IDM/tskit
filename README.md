# tskit — IDM Fork  <img align="right" width="145" height="90" src="https://github.com/tskit-dev/administrative/blob/main/tskit_logo.svg">

> **This is an [Institute for Disease Modeling](https://www.idmod.org/) / [Bill & Melinda Gates Foundation](https://www.gatesfoundation.org/) fork of [tskit](https://github.com/tskit-dev/tskit).**
> It publishes as **`idm_tskit`** on PyPI and adds the `idm` package for high-performance IBD/IBS genome similarity calculations.

## IDM Extensions (`idm` package)

The `idm` package extends tskit with SIMD-accelerated (SSE3/AVX2) Identity by Descent (IBD) and Identity by State (IBS) calculations across large populations of genomes.

## Project status

EMOD-Hub projects are provided as open source software under the MIT License for
community use, research, and development.

**Unless otherwise noted, these projects are no longer actively maintained or supported
by IDM or the Gates Foundation.**

Community contributions are welcome, and trusted collaborators may review and
merge pull requests, but no guarantees are made regarding support, pull request
review, security response, maintenance, or release timelines.

### Install

```bash
pip install idm_tskit
```

### Quick start

```python
import tskit, idm

ts = tskit.load("tree-sequence.ts")

# Extract genome data from the tree sequence
genomes, lengths = idm.get_genomes(ts)

# IBD: similarity by shared ancestral root (pass intervals)
ibd = idm.IbxResults(genomes, intervals=lengths)
score = ibd[genome_a, genome_b]          # raw base-pair overlap
normalized = score / lengths.sum()       # fraction in [0, 1]

# IBS: similarity by matching alleles (omit intervals)
ibs = idm.IbxResults(genomes)
score = ibs[genome_a, genome_b]          # raw site count
normalized = score / genomes.shape[1]   # fraction in [0, 1]
```

For a subset of genomes, pass `indices=np.asarray(ids, dtype=np.uint32)` to reduce computation and memory.

See [IDMEXT.md](IDMEXT.md) for detailed documentation, worked examples, and a description of the SHA256-based deduplication optimization that avoids O(N²) comparisons when clonal genomes are common.

---

[![License](https://img.shields.io/github/license/tskit-dev/tskit)](https://github.com/tskit-dev/tskit/blob/main/LICENSE)
[![Contributors](https://img.shields.io/github/contributors/tskit-dev/tskit)](https://github.com/tskit-dev/tskit/graphs/contributors)
[![Commit activity](https://img.shields.io/github/commit-activity/m/tskit-dev/tskit)](https://github.com/tskit-dev/tskit/commits/main)
[![Coverage](https://codecov.io/gh/tskit-dev/tskit/branch/main/graph/badge.svg)](https://codecov.io/gh/tskit-dev/tskit)
![OS](https://img.shields.io/badge/OS-linux%20%7C%20OSX%20%7C%20win--64-steelblue)


Succinct tree sequences are a highly efficient way of storing a set of related DNA sequences by encoding their ancestral history as a set of correlated trees along the genome. The tree sequence format is output by a number of software libraries and programs (such as [msprime](https://github.com/tskit-dev/msprime), [SLiM](https://github.com/MesserLab/SLiM), [fwdpp](http://molpopgen.github.io/fwdpp/), and [tsinfer](https://tsinfer.readthedocs.io/en/latest/)) that either simulate or infer the evolutionary history of genetic sequences.

The `tskit` library provides the underlying functionality used to load, examine, and manipulate tree sequences, including efficient methods for calculating genetic statistics. It often forms part of an installation of other software packages such as those listed above. Please see the [documentation](https://tskit.readthedocs.io/en/latest/) for further details, which includes [installation instructions](https://tskit.readthedocs.io/en/latest/installation.html).

`tskit` has both a Python and C API


#### Python API
[![PyPI version](https://img.shields.io/pypi/v/tskit.svg)](https://pypi.org/project/tskit/)
[![Supported Python Versions](https://img.shields.io/pypi/pyversions/tskit.svg)](https://pypi.org/project/tskit/)
[![Wheel](https://img.shields.io/pypi/wheel/tskit)](https://pypi.org/project/tskit/)
[![Black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Travis](https://img.shields.io/travis/tskit-dev/tskit)](https://travis-ci.org/github/tskit-dev/tskit)

Most users of `tskit` will use the python API as it provides a convenient, high-level API to access, analyse and create tree sequences. Full documentation is [here](https://tskit.readthedocs.io/en/latest/python-api.html).   

#### C API
[![C99](https://img.shields.io/badge/Language-C99-steelblue.svg)](https://en.wikipedia.org/wiki/C99)
[![CircleCI](https://circleci.com/gh/tskit-dev/tskit.svg?style=shield)](https://circleci.com/gh/tskit-dev/tskit)

The `tskit` C API provides comprehensive, low-level methods for manipulating and processing tree-sequences. Written to the C99 standard and fully thread-safe, it can be used with either C or C++. Full documentation is [here](https://tskit.readthedocs.io/en/latest/c-api.html).


## Disclaimer

The code in this repository was developed by IDM and other collaborators to support our
joint research on flexible agent-based modeling. We've made it publicly available under
the MIT License to provide others with a better understanding of our research and an
opportunity to build upon it for their own work. We make no representations that the code
works as intended or that we will provide support, address issues that are found, or accept
pull requests. You are welcome to create your own fork and modify the code to suit your own
modeling needs as permitted under the MIT License.
