
# Interfacing Buzzer with 8051 Microcontroller (AT89C51)
## Aim

To interface a buzzer with the 8051 (AT89C51) microcontroller and control the buzzer using a push-button switch.

## Apparatus Required

| S. No. | Component | Quantity / Detail |
|---:|---|---|
| 1 | AT89C51 / AT89S52 Microcontroller | 1 |
| 2 | Proteus Professional | Software |
| 3 | Keil µVision IDE | Software |
| 4 | BC547 NPN Transistor | 1 |
| 5 | 330 Ω Resistor | 1 |
| 6 | Push Button Switch | 1 |
| 7 | Buzzer | 1 |
| 8 | 0.1 µF Capacitor | 1 |
| 9 | Power Supply (+5 V) | 1 |

## Pin Connections

| Component | 8051 Pin | Description |
|---|---|---|
| Push Button | P1.4 | Digital input |
| Buzzer Driver | P3.2 | Digital output to BC547 base through 330 Ω |
| BC547 | Collector → Buzzer, Emitter → GND | Switching transistor |
| Buzzer | +5 V and BC547 Collector | Audible output |
| Reset | RST | Reset network |

## Theory

The 8051 microcontroller cannot directly drive most buzzers because of current limitations. A BC547 transistor is used as a switching device. When Port 3.2 becomes HIGH, the transistor turns ON and current flows through the buzzer, producing sound. When Port 3.2 is LOW, the transistor turns OFF and the buzzer remains silent.

## Algorithm

~~~
1. Start the program.
2. Configure P1.4 as input and P3.2 as output.
3. Read the push-button status continuously.
4. If P1.4 = 1, make P3.2 = 1 to turn ON the buzzer.
5. If P1.4 = 0, make P3.2 = 0 to turn OFF the buzzer.
6. Repeat continuously.
~~~

## Circuit Diagram

*(Insert circuit diagram image here — e.g. `![Circuit Diagram](./images/buzzer_8051_circuit.png)`)*

Figure 1. Buzzer interfacing with 8051 microcontroller in Proteus.

## Program (8051 Assembly)

~~~asm
        ORG 0000H
        CLR P1.4
BACK:   CLR P3.2
WAIT:   JNB P1.4, WAIT
        SETB P3.2
        ACALL DELAY
WAIT1:  JB P1.4, WAIT1
        SJMP BACK

DELAY:
        MOV R0, #255
HERE1:  MOV R1, #255
HERE:   DJNZ R1, HERE
        DJNZ R0, HERE1
        RET
        END
~~~

## Program Explanation

| Instruction | Function |
|---|---|
| `ORG 0000H` | Places the program starting address at memory location 0000H. |
| `CLR P1.4` | Initially clears the switch input pin P1.4. |
| `CLR P3.2` | Turns OFF the buzzer connected to P3.2. |
| `JNB P1.4, WAIT` | Waits while P1.4 is LOW. The program continues when the switch input becomes HIGH. |
| `SETB P3.2` | Makes P3.2 HIGH and turns ON the buzzer. |
| `ACALL DELAY` | Calls the delay subroutine. |
| `JB P1.4, WAIT1` | Keeps checking the input while P1.4 remains HIGH. |
| `SJMP BACK` | Returns to the beginning and turns OFF the buzzer after the switch is released. |
| `DJNZ R1` / `DJNZ R0` | Creates a software delay using two nested loops. |
| `RET` | Returns from the delay subroutine. |
| `END` | Marks the end of the assembly program. |

## Procedure

1. Create a new 8051 project in Keil µVision and select the AT89C51/AT89S52 device.
2. Create an ASM source file and enter the given assembly program.
3. Build the project and generate the HEX file.
4. Open the supplied Proteus circuit and double-click the microcontroller.
5. Load the generated HEX file into the Program File field.
6. Set a suitable crystal frequency and run the simulation.
7. Operate the switch connected to P1.4 and observe the buzzer connected through the BC547 transistor.

## Circuit Diagram
<img width="1915" height="1033" alt="image" src="https://github.com/user-attachments/assets/9033306f-c631-4ef7-85ff-bd5c0807b0de" />


## Result

When the push button connected to P1.4 is pressed, the buzzer sounds. When the push button is released, the buzzer stops. The Proteus simulation verifies successful interfacing of the buzzer with the 8051 microcontroller.
