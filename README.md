# eccl
An Electrical Circuit Construction Language parser.

`eccl` is a tool for describing electrical circuits in a non-painful way, without
having to use a graphical editor such as Altium Designer, KiCAD, or Eagle CAD.
All circuit description is done in a text editor. This makes certain things
easier, such as:

* tracking changes to schematics in version control
* applying transformations to circuits using a text parsing program
* creating modular circuits


## Example

For example, the following `eccl` code describes a simple RC circuit.

```
from platonic import res, cap, voltage

block simpleLowPassFilter {
  # Creates a simple low pass filter circuit.
  gnd = pin('GND', symbol: 'GND')
  out = pin('Out', symbol: 'lollipop')

  voltage('V1', ac: 3V, f: 2kHz) | 
    res('R1', value: 1k) -> out -> cap('C1', value: 0.1u)

  $('V1')['-'] -> gnd
}
```

This should compile into a JSON part description and net list, like so:

```javascript
{
  "name": "simpleLowPassFilter",
  "pins": [
    {
      "id": 1,
      "name": "GND",
      "net": 1,
      "symbol": "lollipop"
    },
    {
      "id": 2,
      "name": "Out",
      "net": 2,
      "symbol": "lollipop"
    }
  ],
  "parts": [
    {
      "id": 1,
      "name": "V1",
      // here lies anything imported from platonic.voltage
      "props": {
        "ac": 3.0,
        "f": 2000.0,
      },
      "nets": [3, 1]
    },
    {
      "id": 2,
      "name": "R1",
      // here lies anything imported from platonic.res
      "props": {
        "value": 1000.0
      },
      "nets": [3, 2]
    },
    {
      "id": 3,
      "name": "C1",
      // here lies anything imported from platonic.cap
      "props": {
        "value": 0.00000001 
      },
      "nets": [2, 1]
    },
  ]
}
```
## Aims

* Allow the user to specify part details as late as possible. Instead of using
  parts from a library that include values, footprints, tolerances and so on, 
  add details to parts as you figure them out.

* Allow test-driven circuit development. Let the user start from a set of
  circuit tests, and then create the circuit to pass them.

* Allow composition of circuits. Make circuit blocks re-usable and modular as
  possible.

* Facilitate simulation and visualisation. Make it easy to create simulation
  tools that take advantage of all the user's knowledge about the circuit. 

## Language Specification

The language spec will evolve, and you can track its evolution in `doc/spec.md`.
Of course there are numerous exciting future features being dreamed up, and you
can read all about them in `doc/upcoming.md`. 

## Parser

The parser will be written in Javascript, using the tutorial laid
[here](http://javascript.crockford.com/tdop/tdop.html). The parser aims to be
correct according to `doc/spec.md`.

## Visualiser

The visualiser will also be written in Javascript, possibly as a set of Vue
components. Plan TBD.


