# Acquisition geometry specification


# Specification

We specify acquisition geometries using a [TOML](https://github.com/toml-lang/toml/blob/master/versions/en/toml-v0.4.0.md) file. A minimal example is given here:

```toml
specifies = "geometry"
type = "parallel"
dimension = 3

[volume]
origin = [0.0, 0.0, 0.0]
lengths = [10.0, 10.0, 10.0]
shape = [100, 100, 100]

[parameters]
source-distance = 5.0
detector-distance = 5.0
projection-count = 10
# angles: [0, 0.5, 1.0, ...]
# ...
```

The geometry specs have the following structure: first there is a *header*, then the *volume* is defined, and finally the *parameters* for the geometry are given.

## Header

- First it is specified that the file is a geometry specification, to distinguish it from other specifications:

    ```toml
    specifies = "geometry"
    ```

- In the second entry the *type* (or *class*) of the geometry as:

    ```toml
    class = "name"
    ```

    Where `name` is one of the following:
    - `parallel`
    - `dual-axis-parallel`
    - `cone-beam`
    - `fan-beam`
    - `laminography`
    - `tomosynthesis`
    - `trajectory`

- Finally the problem dimension is given as:

    ```toml
    dimension = 3
    ```

The complete header looks like e.g.:

For 3D parallel beam:

```toml
specifies = "geometry"
type = "parallel"
dimension = 3
```

For 2D fan beam:

```toml
specifies = "geometry"
type = "fan-beam"
dimension = 2
```

etc.

## Volume

After the header, the *volume* table is given. We make a

The table has *three* entries:
- `origin`: an array of `D` floating point numbers. The left-most point of the volume (in each axis) in physical coordinates.
- `lengths`: an array of `D` floating point numbers. The size of the volume in physical coordinates.
- `shape`: an array of `D` integers. The number of discretization points for each axis.

A valid volume definition is e.g.:

```toml
[volume]
origin = [0.0, 0.0, 0.0]
lengths = [10.0, 10.0, 10.0]
shape = [100, 100, 100]
```

This defines a volume of physical size `10 x 10 x 10`, in the *positive octant* in physical coordinates, that consists of `100^3` discretization points.

## Parameters

Next a table of parameters are given. These depend on the geometry and are outlined below

### Parallel

The parallel geometry defines a parallel beam setup where the object is rotated along the physical z-axis.

- `source-position`: an array of `D` floating point numbers. The position of the source in physical coordinates.
- `detector-position`: an array of `D` floating point numbers. The position of the detector in physical coordinates.
- `detector-tilt`: an array of two arrays of `D` floating point numbers. The (initial axes) of the detector.
- (optional) `projection-count`: an integer. The number of projections.
- (optional) `angles`: a list of floating point numbers. The rotation angle for each projection.

Either `projection-count` or `angles` have to be given. If they are both given, then the length of `angles` should be equal to `projection-count`. If only `projection-count` is given then the angle list will correspond to an equipartitioning of the interval `[0, pi)`.
