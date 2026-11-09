# K1V2SPI Programmer

Programmer for **1.2 V SPI NOR flash**. A PC GUI talks over USB to a **PIC18F16Q20 Curiosity Nano**, which drives the flash through a level translator.

```
PC GUI --USB--> PIC18F16Q20-CNANO --SPI 3.3 V--> level translator --SPI 1.2 V--> SPI NOR flash
```

**Supported flash** (identified automatically from the JEDEC ID and JESD216H SFDP): Macronix MX25S1633F, GigaDevice GD25UF16E, Winbond W25Q65NE and W25Q32NE. Other parts can be added in the GUI with *Custom Flash*.

## Quick start

1. **Load the firmware** into the Nano (USB-C only, no flash connected):
   Program `firmware/prebuilt/K1V2SPI_FW_PIC18F16Q20.hex` with MPLAB IPE (device `PIC18F16Q20`, tool: the Nano's on-board debugger), or run `firmware\flash_nano.bat`, which does this with `ipecmd` and then verifies by read-back.
   In a plain clone the verification step cannot run (see *Building from source*): the board is still programmed, but the script ends with an error after the read-back.
2. **Wire** the Nano, level translator, 1.2 V supply and flash as described in the User Guide.
   The flash must never see 3.3 V.
3. **Run the GUI**, select the Nano's serial port and click *Connect*:

   | OS | Program |
   |---|---|
   | Windows | `gui/Win/dist/K1V2SPI_Programmer.exe` |
   | Ubuntu 22.04+ | `gui/Linux/dist/K1V2SPI_Programmer` (add yourself to the `dialout` group once: `sudo usermod -aG dialout $USER`, then log in again) |

No Python installation is needed.

## Documentation

The **User Guide** covers the hardware (architecture, wiring, BOM, loading the firmware), the serial protocol and the GUI:

- [English](<User Manual/K1V2SPI Programmer User Guide EN.pdf>)
- [繁體中文](<User Manual/K1V2SPI Programmer User Guide CHT.pdf>)

## Repository layout

| Path | Content |
|---|---|
| `firmware/` | Bridge firmware source (XC8), prebuilt HEX, build and flash scripts |
| `gui/` | GUI source (Python/tkinter); `Win/` and `Linux/` hold the build scripts and executables |
| `Hardware/` | Altium project, schematic PDF, block diagram and BOM |
| `User Manual/` | User Guide PDFs |

**Building from source.** `gui/Linux/build_linux.sh` (Ubuntu) and `gui/Linux/build_linux_docker.bat` (Ubuntu build from Windows, with Docker) work from this repository alone.
`gui/Win/build_win.bat` and the read-back verification step of `firmware/flash_nano.bat` call a helper (`Private/tools/py.cmd`) that is kept in a separate private repository, so they do not work in a plain clone.
To run the GUI from source instead: `pip install -r gui/requirements-dev.txt`, then `python gui/App_GUI.py`.

## Versions

GUI v5.4.1 · bridge firmware v5.1.1

## Contact

Keynes Hsu — keynes.hsu@microchip.com
