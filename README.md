# OASIS libcamera CMake wrapper

This implements a thin CMake wrapper around the [libcamera](https://libcamera.org) meson project. It only builds the main library without examples, tests or documentation.

```sh
cmake -B build -GNinja
cmake --build build
```

The number of parallel processes can be limited by setting `NJOBS`:
```sh
cmake -B build -GNinja -DNJOBS=2
cmake --build build
```

When using this as part of a colcon workspace, `NJOBS` can be set via `--cmake-args`:
```sh
colcon build --cmake-args="-DNJOBS=2"
```

## Dependencies

To avoid errors during `colcon build`, make sure you have installed the right dependencies, for Ubuntu:

`apt-get update && apt-get upgrade -y && apt-get install -y v4l-utils python3 python3-pip git python3-jinja2 libboost-dev libgnutls28-dev openssl libtiff5-dev pybind11-dev qtbase5-dev libqt5core5a libqt5gui5 libqt5widgets5 cmake python3-yaml python3-ply libglib2.0-dev libgstreamer-plugins-base1.0-dev ninja-build libevent-dev`
