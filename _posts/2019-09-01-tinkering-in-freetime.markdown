---
layout: post
title:  "Tinkering on pyproj: The CRS class"
date:   2019-09-01 23:01:06 -0500
categories: open-source pyproj crs
---

Ever since my beginnings with the python open source ecosystem, I have constantly
run into cases where the input format from a Coordinate Reference System (CRS) is
stored in either a PROJ string, a WKT string, an EPSG code, and the list goes on.
And then based on that, I usaully need to convert it to another format for some
purpose or another because a different software package requires it. In the past,
the best resource for this task was GDAL. And of course this meant you had to
install GDAL, which has been and continues to be a challenge for many.

Thanks to the [GDAL Barn Raising work](https://gdalbarn.com/), this part of the GDAL
code has been merged with the original PROJ.4 code into the PROJ 6+. This means that the
only piece you need to install is PROJ 6+. Although that sounds simple, it will remain a
challenge if you use other libraries until the open source ecosystem fully adopts it (see [adoption status](https://github.com/OSGeo/PROJ/wiki/proj.h-adoption-status)). That being said, thanks to conda-forge and wheels, installation is a much easier task and the barrier
to adoption is much lower.

After recognizing the potential, I decided to tinker around in my free time and follow the great work of the CRS classes created by the great python geospatial programmers that came before me such as the ones in [rasterio](https://github.com/mapbox/rasterio) and [opendatacube](https://github.com/opendatacube/datacube-core). With the assistance from the PROJ developers [Even Rouault](https://github.com/rouault) and [Kristian Evers](https://github.com/kbevers), I was able to navigate the new PROJ C-API and add the features needed. The result was the [pyproj.CRS](https://pyproj4.github.io/pyproj/stable/api/crs.html#pyproj-crs) class.
This has been available since pyproj 2.0.0 thanks to [Jeffrey Whitaker](https://github.com/jswhit) accepting the PR and allowing it to be part of pyproj. Since then, many great ideas from [Joris Van den Bossche](https://github.com/jorisvandenbossche) have lead to the `pyproj.CRS` class to further improve.

There are some examples of usage in the `pyproj` API documentation as well as in the [Getting Started](https://pyproj4.github.io/pyproj/stable/examples.html) pages.


Here is an example of using the `pyproj.CRS` class and initializing from a URN.

```python
>>> from pyproj import CRS
>>> urn_crs = CRS("urn:ogc:def:crs:OGC::CRS84")
>>> urn_crs
<Geographic 2D CRS: OGC:CRS84>
Name: WGS 84 (CRS84)
Axis Info [ellipsoidal]:
- Lon[east]: Geodetic longitude (degree)
- Lat[north]: Geodetic latitude (degree)
Area of Use:
- name: World
- bounds: (-180.0, -90.0, 180.0, 90.0)
Datum: World Geodetic System 1984
- Ellipsoid: WGS 84
- Prime Meridian: Greenwich

>>> urn_crs.to_authority()
('OGC', 'CRS84')
```

I learned quite a bit along the way and tried to highlight the important CRS gotchas I have learned along this journey in the [pyproj Gotchas/FAQ documentation](https://pyproj4.github.io/pyproj/stable/gotchas.html).
