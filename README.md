# eccl
An Electrical Circuit Construction Language parser.

eccl is a tool for describing electrical circuits in a non-painful way, without
having to use a graphical editor such as Altium Designer, KiCAD, or Eagle CAD.
All circuit description is done in a text editor. This makes certain things
easier, such as:

* tracking changes to schematics in version control
* applying transformations to circuits using a text parsing program
* creating modular circuits


## Example

For example, the following `eccl` code should produce this schematic:

```
from platonic import res, cap, voltage

block simpleLowPassFilter {
  # Creates a simple low pass filter circuit.
  pin gnd, out
  
  voltage('V1', ac: 3V, f: 2kHz) | 
    res('R1', value: 1k) -> out -> cap('C1', value: 0.1u)

  $('V1')['-'] -> gnd
}
```

(Todo: actually make the schematic.)


