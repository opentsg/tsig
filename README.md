# tsig - Test Signal Input Geometry

The **Test Signal Input Geometry** tool allows you to generate 3d displays in
[obj][o1] and [TSIG][t2] formats. These can be passed to [opentsg][otsgw] to
create mathematically correct pixel mapping to calibrate and test displays in
virtually any color space and any shape.

This repo contains a number of demonstration shapes and the template code to
allow you to create your own custom shapes. In this demo, we have some shapes
that can easily be made by arranging square LED tiles into larger panels:

- An open cube (No front wall panel)
- A curved cylindrical wall (fixed radius in x & y planes, straight z plane)
- A spherical cap display fixed radius in x & y & z planes)

## Getting started

Make sure you are familiar with [OpenTSG][t1] and what [TSIGs][t2] are and how
they are used.

In short TSIGs allow you to generate test patterns for 3d displays, with pixel
accurate mapping. These TSIGs are used as an input for OpenTSG, which generates
the test signal.

Please note every obj file produced has tha z axis as the vertical axis, so
when importing the obj into your software of choice you can get the object the
right way up.

## Installation

Make sure you have the latest version of Go from the [official golang
source][g1] installed.

Then run the go build command to compile the code.

```cmd
go build
```

## Commands

```sh
./tsig --help
```

This will list the available commands. In general, execute commands of the form

```sh
./tsig --conf ./path/to/geometry.yaml --outputFile ./path/to/output
```

The default command `gen` generates both the obj and TSIG files.

The `obj` command ensures only an obj file is written.

The `tsig` command ensures only a TSIG tile is written.

The `list` command list the available shapes and their brief descriptions.

## Flags

### Generate flags

These flags work for every obj and TSIG generating command

The `--conf` flag tells the cli which input file to parse.

The `--outputFile` tells the name of the file(s) to be saved, the input string
is without the inclusion of the file extension. e.g.  `--outputFile
./examples/example` will produce two files, `./examples/example.obj` and
`./examples/example.json`

### list flags

To be added

## Demos

Ensure the command line is installed and running with `./tsig --help` The
name of the program may change based on your operating system.

Then you can run the first demo, which will generate a cube obj and TSIG. This
will make 2 separate files, of one obj and one json TSIG file. Run the
following command

```cmd
./tsig --conf ./examples/cube.yaml --outputFile ./examples/cube
```

Now to find the output at the `./examples` folder you will have generated
`./examples/cube.obj` and `./examples/cube.json`. Congratulations on making
your first TSIG!

### Obj only Demo

If you just want to make the obj then you can specify that with the `obj`
command. Run the following code below to just make the cube obj.

```cmd
./tsig obj --conf ./examples/cube.yaml --outputFile ./examples/ObjOnly
```

If you check the `./examples` folder you will have generated
`./examples/ObjOnly.obj`, there is no TSIG in sight.

### TSIG Only Demo

If you just want to make the TSIG then you can specify that with the `tsig`
flag. Run the following code below to just make the cube TSIG.

```cmd
./tsig tsig --conf ./examples/cube.yaml --outputFile ./examples/tsigOnly
```

If you check the `./examples` folder you will have generated
`./examples/tsigOnly.json`, there is no obj in sight.

### Available shapes

The following list of shapes will take you through the config file for that
shape. The config files can be in json or yaml, the field names will be the
same for both file types.

- [Cube][cbd]
- [Curve][cvd]
- [Spherecap][spd]

Once a demo has been run, the TSIG output can be plugged into openTSG.

### Cube Demo

This demo will walk you through generating a cube shape display, with a missing
front panel. There is no option to add the front panel in.

The cube demo is run with an input file of `./examples/cube.yaml`
which looks like.

```yaml
---
# The file type identifier
shape: cube
# tile dimensions in abstract units
tileHeight: 0.5
tileWidth: 0.5
# cube dimensions
# X dimensions
cubeWidth: 5
# Z dimensions
cubeHeight: 5
# Y dimension
cubeDepth: 2.5
# Pixels per tile
# dx matches the tile width
dx: 500
# dy matches the tile height
dy: 500
```

Every field is required.

Run the following to generate the cube TSIG and obj files.

```cmd
./tsig --conf ./examples/cube.yaml --outputFile ./examples/cube
```

The generated files will be in `./examples` as `./examples/cube.obj` and
`./examples/cube.json`. Plug the obj into an obj visualiser and see how it
looks. Then try using  the TSIG with openTSG to generate a pattern of your
choosing.

Feel free to change any of the values in the file and run it again, try
changing the depth to make the cube really shallow.

### Curve Demo

This demo will walk you through generating a curved wall display.

The curve demo is run with an input file of `./examples/curve.yaml` which looks
like.`

```yaml
---
# The file type identifier
shape: curve
# tile dimensions
tileHeight: 0.5
tileWidth: 0.5
# cylinder dimensions
cylinderRadius: 5
cylinderHeight: 5
# Max angle in radians
# will be 30 degrees either side of the azimuth
# in this example.
azimuthMaxAngle: 0.5235987755982988
# Pixels per tile
dx: 500
dy: 500

```

Every field is required

Run the following to generate the curve obj and TSIG files.

```cmd
./tsig --conf ./examples/curve.yaml --outputFile ./examples/curve
```

The generated files will be in `./examples` as `./examples/curve.obj` and
`./examples/curve.json`. Plug the obj into an obj visualiser and see how it
looks. Then try using  the TSIG with openTSG to generate a pattern of your
choosing.

Feel free to change any of the values in the file and run it again, change the
angle and see how the uv map changes.

### Spherecap Demo

This demo will walk you through generating a spherecap wall display.

The spherecap demo is run with an input file of `./examples/SphereCap.yaml`
which looks like.

```yaml
---
# The file type identifier
shape: spherecap
# tile dimensions
tileHeight: 0.5
tileWidth: 0.5
# Sphere dimensions
radius: 5
# Max angle in radians
# will be 30 degrees either side of the azimuth
# in this example, and 30 degrees either side of
# the inclination (theta).
thetaMaxAngle: 0.5235987755982988
azimuthMaxAngle: 0.5235987755982988
# pixel change per tile
dx: 500
dy: 500
```

Run the following to generate the spherecap files.

```cmd
./tsig --conf ./examples/SphereCap.yaml --outputFile ./examples/spherecap
```

The generated files will be in `./examples` as `./examples/spherecap.obj` and
`./examples/spherecap.json`. Plug the obj into an obj visualiser and see how it
looks. Then try using  the TSIG with openTSG to generate a pattern of your
choosing.

Feel free to change any of the values in the file and run it again, change the
angles, try making a wide view spherecap screen by increasing
`azimuthMaxAngle:` to 1.309.

## Golden ratios

Any numbers that seem to work really well.<br>
e.g. dx* tile width / radius must =5

## Technical stuff

### Developer notes

Make your self familiar with the obj format [here][o1].

Each tile will follow this sort of design in the code,
where v is the xyz coordinate of the vertex, and vt is the
uv coordinate of the texture.

```obj
v 0 0 0
v 0.5 0 0
v 0.5 0 0.5
v 0 0 0.5
vt 1 0.25
vt 0.95 0.25
vt 0.95 0.3
vt 1 0.3
f 1/1 2/2 3/3 4/4
```

That tile would look like this in a TSIG.

```json
{
  "Name": "",
  "Tags": null,
  "Neighbours": null,
  "Layout": {
    "Carve": {
      "X": 0,
      "Y": 0
    },
    "Flat": {
      "X": 9500,
      "Y": 7000
    },
    "XY": {
      "X": 500,
      "Y": 500
    }
  }
}
```

Where the flat coordinate relates to this uv map position `vt 0.95 0.25`, and
the rest of the coordinates are found from the `"XY"` field that gives the
height and width of the tile.

Cube uv map design.

Design thoughts
formulas used.
The sphere uv map

[g1]:   https://go.dev/doc/install                "Golang Installation"

[t1]:   https://opentsg.studio/                    "openTSG Website"
[t2]:   https://github.com/mrmxf/opentsg-node/blob/main/READMETPIG.md            "TSIG information"

[o1]:   https://en.wikipedia.org/wiki/Wavefront_.obj_file    "OBJ wikipedia"

[cbd]: #cube-demo
[cvd]: #curve-demo
[spd]: #spherecap-demo

[otsgg]:  https://github.com/opentsg/
[otsgw]:  https://opentsg.studio
