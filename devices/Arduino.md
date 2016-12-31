# Arduino


## Configure IDE
Tools  
Port: `com-` (Windows)  
Board: "Arduino Nano" ATMega328  
Board: "Arduino Uno" ATmega328P  

If using Mac OS and no port can be selected:
Install signed CH340 drivers

## Security rules

- Never connect VCC (+5V) and GND without a resistor (min 125 Ohm) in the middle
- Detach the USB cable when changing the circuit
- Multimeter: When unsure about the scale, start with the highest

## Basics

### Standards

- Anode (A) is the part of a component that connect to VCC
- Cathode (K) is the part of a component that connects to GND
- Standard wire colors:
  - Red: VCC
  - Black: GND
- On buildings
  - Black: Line/Phase
  - Blue: Neutral
  - Yellow-Green: Protective Earth

Breadboard:
- In the blue/black rows and the red rows, the holes are connected horizontally
- In the rest of the board, the holes are connected vertically

### Physics

| Letter | Concept    |
|--------|------------|
| I      | Current    |
| R      | Resistance |
| V      | Voltage    |

```
V = R x I   <=>   R = V / I
```

(la **V**ache qui **RI**t)

#### Baud rate / Bit rate
Number of transferred bits per second in serial communication


## Components

### Resistors

Color code:
http://www.researchcell.com/wp-content/uploads/2012/05/resistor-color-code-band.jpg

| Color  | Value |
|--------|-------|
| Black  | 0     |
| Brown  | 1     |
| Red    | 2     |
| Orange | 3     |
| Yellow | 4     |
| Green  | 5     |
| Blue   | 6     |
| Violet | 7     |
| Grey   | 8     |
| White  | 9     |
| Gold   | 5%    |
| Silver | 10%   |

My resistors:
270 (Red Violet Brown) Many
330 (Orange Orange Brown) One


### Single Color LED
- LED longer leg is Anode, the shorter is the Cathode
- Voltage around 2V
- Amperage around 20mA
- `5V - 2V = R * 0.02A <=> R = 150 Ohm`

### RGB LEDs
- Longer leg is Cathode (K)
- Order is RKBG

### Button
- Legs should be placed on the top and bottom sides
- When clicked, connects the left and the right sides

### 4-way Switch
- Always place in the empty row of the breadboard
- Connects bottom pin to top pin when corresponding switch is ON

### Piezzo Speaker
- No special resistor needed, so use a 125Ohm.


## Software
- Programs run `setup()` once and then iterate `loop()` forever


### Pins
Can be set as `INPUT` or `OUTPUT`.

#### Digital output

Can be set to 0V (`LOW`) or +5V (`HIGH`)
Pin13 is the status LED (L) in the board itself.

#### Pulse width modulation (PWM)

Simulates analog output on the digital pins. Duty cycle means the percentage of HIGH.  
`analogWrite` values from 0 to 255.

#### Analog input

Can be `analogRead` from 0 to 1023.




