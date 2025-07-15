# $\delta$ Scuti Pulsation Model Grid

This repository contains information regardingthe $\delta$ Scuti star pulsation model grid associated with our [paper](https://arxiv.org/abs/2507.03561).

## Data Access

The complete model grid is available on [Zenodo](https://zenodo.org/records/15839323).

## Grid Description

The full modelling methodology is described in our [paper](https://arxiv.org/abs/2507.03561). The grid is generated using the [MESA](https://docs.mesastar.org/en/latest/) stellar evolution code and the [GYRE](https://gyre.readthedocs.io/en/stable/) pulsation code. The specific python scripts used to modify the inlists and generate the grid will be made available on request.

The grid contains the following parameters:

### Basic Stellar Model Parameters
- `i`: index
- `m`: Star mass
- `z`: Initial metal mass fraction
- `v`: Initial rotation rate (km/s)
- `Myr`: Age
- `phase_of_evolution`: Phases of evolution as defined in our [paper](https://arxiv.org/abs/2507.03561):
  - PreMS : 2
  - From ZAMS to 50 Myr : 3 
  - MS : 4
  - From TAMS to the begining of the subgiant phase : 5

### Physical Properties
- `teff`: Effective temperature (K)
- `log_L`: log Luminosity - log10(L/LSun)
- `log_R`: log Radius - log10(R/RSun)
- `density`: Average stellar density
- `log_g`: log surface gravity

### Asteroseismic Parameters
- `Dnu`: Large frequency spacing, Delta nu
- `eps`: Epsilon, radial ridge y-intercept

### Pulsation Modes
- `n{n}ell{l}m0`: p-mode frequency for mode with n (= 1 to 11), ell (= 0 to 3), m = 0
- `n{n}ell{l}dfreq`: p-mode rotational splitting for mode with n (= 1 to 11), ell (= 0 to 3), m = 0
- `ng0ell{l}m0`: f-mode frequency for ell (= 2 and 3), m = 0
- `ng0ell{l}dfreq`: f-mode rotational splitting for ell (= 2 and 3), m = 0
- `ng{n}ell{l}m0`: g-mode frequency for mode with n (= 1 to 5), ell (= 1 to 3), m = 0
- `ng{n}ell{l}dfreq`: g-mode rotational splitting for mode with n (= 1 to 5), ell (= 1 to 3), m = 0

### Rotation
- `R_eq`: Radius at the equator (flattening)
- `R_polar`: Radius at the pole (flattening)
- `omega`: Rotation rate in cycles/day
- `omega_c`: Critical rotation rate in cycles/day
- `v_eq`: Surface average rotation rate in km/s

### Energy Generation
- `log_LH`: log Luminosity from H-burning
- `log_LHe`: log Luminosity from He-burning
- `pp`: log Luminosity from p-p chain reaction
- `cno`: log Luminosity from CNO cycle

### Gravity Darkening
- `grav_dark_L_polar`: log Luminosity from gravity darkening at the pole
- `grav_dark_Teff_polar`: log Teff from gravity darkening at the pole
- `grav_dark_L_equatorial`: log Luminosity from gravity darkening at the equator
- `grav_dark_Teff_equatorial`: log Teff from gravity darkening at the equator

## Reading the grid

The grid is in parquet format. You can read it with the following code if there is enough memory on your machine:

```python
import pandas as pd

df = pd.read_parquet('path/to/grid.parquet')
```

Since the grid is quite large, if there is not enough memory on your machine, you can read the grid in required chunks. For example, to read specific columns of the grid for the pre-main sequence phase, you can do:

```python
import pandas as pd

df = pd.read_parquet('path/to/grid.parquet', filters=[('phase_of_evolution', '==', 2)], columns=['i', 'm', 'z', 'v', 'Myr', 'teff', 'log_L', 'density', 'Dnu', 'eps'])
```

Here, the `filters` argument is used to query specific rows of the grid. Multiple filters can be applied as an AND condition at the same time. The `columns` argument is used to select specific columns.