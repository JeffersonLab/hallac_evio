# evio Software Package 4.4.6

This is a snapshot of the evio 4.4.6 release as published at:
  https://coda.jlab.org/drupal/system/files/evio-4.4.6.tgz

It is reproduced with permission in this git repository for the purposes
as a build dependence (via submodule) for the Hall A and C analyzers.

The upstream DAQ Group evio repository is private, but can be found here:
  https://github.com/JeffersonLab/evio

At some point we plan to replaces this ancilliary repo with a pointer to a
tagged release off the DAQ group's repo.

-- Brad Sawatzky <brads@jlab.org>

## Cmake Build

```
git clone https://github.com/whit2333/hallac_evio.git
cd hallac_evio
mkdir build && cd build
cmake ../. -DCMAKE_INSTALL_PREFIX=$HOME
make -j4 install
```
These instruction assume that you have `$HOME`  configured, e.g., in your 
`.bashrc`:  (BTW: we need to stop using tcsh by default. **Use BASH** - it is 
2018)

```
export PATH=$HOME/bin:$PATH
export LD_LIBRARY_PATH=$HOME/lib:$HOME/lib64:$LD_LIBRARY_PATH
```

