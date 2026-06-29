# domo-2040
A custom microcontroller built around the RP2040

<img width="415" height="997" alt="image" src="https://github.com/user-attachments/assets/ba2fadf9-0230-4eb2-ad65-4ca726dd36a6" />

## Features
* RP2040 microcontroller
* 27 GPIO pins
* 16MB flash memory
* Onboard user button (GPIO24)
* Onboard user LED (GPIO25)
* USB-C connection
* Compatible with Pi Pico footprint
* Super cool decoration
* Cute Domos
## Why?
I use devboards like the Pi Pico all the time, but I never really understood how they worked or anything. Specifically, I was curious as to why projects tend to use premade devboards such as Pi Picos rather than just use a plain RP2040. So naturally, I decided the best way to learn was to make one myself. Also, funnily enough the BOM comes out to about half the retail price of the Pico before countimg one-time assembly fees, which means if I were to theoretically mass produce this me and my friends could get devboards at about half price. 
## BOM
| Component        | Specification | Quantity | LCSC Part # | Price |
|------------------|---------------|----------|-------------|-------|
| Capacitor 0402   | 1uF           | 2        | C52923      | $0.02 |
| Capacitor 0402   | 0.1uF         | 11       | C1525       | $0.06 |
| Capacitor 0402   | 33pF          | 2        | C1562       | $0.02 |
| Capacitor 0603   | 10uF          | 2        | C19702      | $0.20 |
| Diode SOD-323    | Schottky      | 1        | C191023     | $0.02 |
| LED 0603         | White LED     | 1        | C2290       | $0.02 |
| USB C Receptacle | USB C         | 1        | C2765186    | $0.21 |
| Resistor 0402    | 5.1k          | 2        | C25905      | $0.01 |
| Resistor 0402    | 27            | 2        | C25156      | $0.02 |
| Resistor 0402    | 1k            | 3        | C11702      | $0.02 |
| Resistor 0402    | 10k           | 2        | C25744      | $0.01 |
| Switch           | Alps SKRK     | 2        | C720477     | $0.11 |
| MCU              | RP2040        | 1        | C2040       | $2.00 |
| Flash Chip       | 128Mbit       | 1        | C97521      | $2.65 |
| LDO SOT-23       | 5v->3v3       | 1        | C5446       | $0.20 |
| Crystal          | 12MHz         | 1        | C9002       | $0.20 |
| **Soldering Iron** | |             1        |             |$20 |
