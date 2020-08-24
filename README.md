# IGRF {13,12,11} in Python

[![DOI](https://zenodo.org/badge/33064474.svg)](https://zenodo.org/badge/latestdoi/33064474)
![Actions Status](https://github.com/space-physics/igrf/workflows/ci/badge.svg)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/space-physics/igrf.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/space-physics/igrf/context:python)

[![Python versions (PyPI)](https://img.shields.io/pypi/pyversions/igrf.svg)](https://pypi.python.org/pypi/igrf)
[![PyPi Download stats](http://pepy.tech/badge/igrf)](http://pepy.tech/project/igrf)

International Geomagnetic Reference Field: IGRF13, IGRF12 IGRF11...in simple, object-oriented Python &ge; 3.6 or Matlab.

![image](src/igrf/tests/incldecl.png)

![image](src/igrf/tests/vectors.png)

## Install

A Fortran compiler is required, such as `gfortran`:

* Linux: `apt install gfortran`
* Mac: `brew install gcc`
* [gfortran for Windows](https://www.scivision.dev/windows-gcc-gfortran-cmake-make-install/) (MinGW)
   Windows only: from Powershell:

   ```posh
   echo "[build]`ncompiler=mingw32" | Out-File -Encoding ASCII ~/pydistutils.cfg
   ```

To get the IGRF Python development version, `git clone` and then:

```sh
python -m pip install -e .
```

Otherwise, for the latest release from PyPi:
```sh
python -m pip install igrf
```

Optionally, test the install with:
```sh
pytest
```

### Windows
If you get ImportError on Windows for the Fortran module, try from the `iri2016` directory:

```posh
del *.pyd
python setup.py build_ext --inplace --compiler=mingw32
```


## Example
To make the plots in this readme:

```sh
python RunIGRF.py
```

using as a Python module at geodetic coordinates 65N, 148W:

```python
import igrf

mag = igrf.igrf('2010-07-12', glat=65, glon=-148, alt_km=100)
```

returns an `xarray.Dataset`:

```
<xarray.Dataset>
Dimensions:  (alt_km: 1)
Coordinates:
  * alt_km   (alt_km) int64 100
Data variables:
    north    (alt_km) float64 1.122e+04
    east     (alt_km) float64 4.148e+03
    down     (alt_km) float64 5.302e+04
    total    (alt_km) float64 5.436e+04
    incl     (alt_km) float64 77.29
    decl     (alt_km) float64 20.29
```

The IGRF model may be specified with the `igrf.igrf(model=)` option:

* `11`: IGRF11
* `12`: IGRF12
* `13`: IGRF13

### Matlab

Matlab can seamlessly call Python modules, as used in [igrf.m](./+igrf/igrf.m).
Instead of the $1000 Aerospace Toolbox, use this free IGRF for Matlab like:

```matlab
igrf.igrf(datetime(2020,1,3,5,4,22), 20, 60, 0)
```

## Reference

If you only want the plain Fortran program, you can do:

```sh
cmake -B build

cmake --build build --parallel

./testigrf
```

### References

* [IGRF13 Fortran code](http://www.ngdc.noaa.gov/IAGA/vmod/igrf13.f)
* [IGRF12 Fortran code](http://www.ngdc.noaa.gov/IAGA/vmod/igrf12.f)
* [IGRF11 Fortran code](http://www.ngdc.noaa.gov/IAGA/vmod/igrf11.f)
* WMM2015 [inclination map](https://www.ngdc.noaa.gov/geomag/WMM/data/WMM2015/WMM2015_I_MERC.pdf)
* WMM2015 [declination map](https://www.ngdc.noaa.gov/geomag/WMM/data/WMM2015/WMM2015_D_MERC.pdf)
