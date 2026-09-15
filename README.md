# ATmega16 Multi-Function Embedded System

> Three independent ATmega16 (AVR) projects: RTC clock, temperature sensing, music playback,
> touch piano, and infrared remote control. All run at 16 MHz external crystal.

## Projects

| Project | Size | Description |
|---------|------|-------------|
| `mega16` | 21 KB | IR remote decode + DS1302 RTC + DS18B20 temperature + 3-song music + 8-digit 7-segment display + timestamp record/playback |
| `piano` | 2 KB | 8 touch keys (PA0–PA7) + PIR auto-sleep + PD5 buzzer, auto power-off after 180s idle |
| `test` | 12 KB | 5 modes: clock / countdown / stopwatch / temperature / music, 6-digit display, K1–K4 buttons |

## Hardware

- **MCU**: ATmega16 @ 16 MHz
- **Display**: 8-digit common-cathode 7-segment (PORTA digit select, PORTC segment)
- **Temperature**: DS18B20 (1-Wire, PC7/PD7)
- **RTC**: DS1302 (PA6=IO, PA7=SCLK, PA5=RST)
- **IR receiver**: VS1838B on PD2 (INT0), NEC protocol
- **Buzzer**: PD5 (Timer1 CTC)
- **Touch keys**: PA0–PA7, internal pull-up, active low
- **PIR**: PB0 (HC-SR501)

## Music Library

Four built-in tunes via Timer1 CTC:

1. 两只老虎
2. 新年好
3. 菊花台
4. See You Again (simplified)

## Key Techniques

- **IR decode**: INT0 falling edge, microsecond pulse measurement, NEC address/~address check
- **DS1302 RTC**: bit-banged serial protocol, Zeller's congruence for weekday
- **DS18B20**: 1-Wire temperature read in main loop
- **Music**: frequency table → OCR1A reload → CTC toggle on OC1A
- **7-segment scan**: timer interrupt digit multiplexing
- **Key debounce**: non-blocking counter-based, no music blocking

## Build & Flash

1. Open source in CodeVisionAVR, Atmel Studio, or avr-gcc
2. Chip: ATmega16, crystal 16 MHz
3. Compile to `.hex`, flash via USBasp / JTAG
4. Wire peripherals per `piano_circuit.md`

## Notes

- Three projects are standalone source files (no IDE project files)
- 7-segment table is common-cathode; invert for common-anode
- Music playback uses blocking delay; key response may lag during playback
