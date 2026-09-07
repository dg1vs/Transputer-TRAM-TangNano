# Transputer-TRAM-TangNano

A **TRAM-size FPGA Transputer module** based on the **Sipeed Tang Nano 20K**.

The goal of this project is to provide a modern, compact Transputer module which can be used in systems designed for classic INMOS TRAMs, while replacing most of the increasingly difficult-to-obtain historical hardware with an FPGA implementation.

The project targets FPGA implementations of the **T805** and **T425** Transputers and is intended both for practical use in existing Transputer systems and for experimenting with Transputer hardware today.

The main idea behind this project is to provide a TRAM with an FPGA based implementation. For FPGA imlemetation please see the paper [1]. 

## Main Features

- TRAM-size PCB
- Sipeed **Tang Nano 20K** FPGA module
- FPGA implements the **INMOS T805 and T425**
- Standard Transputer link interface
- Four Transputer links
- Support for the usual TRAM control signals
- 5 V / 3.3 V level adaptation between classic Transputer hardware and the FPGA
- I²S audio support
- Selected Transputer and FPGA signals available on pin headers
- Can be used in systems with standard TRAM slots, including the **ATW800/2**
- Ready-to-program Gowin `.fs` FPGA image available

## Background

Original Transputer systems are still interesting platforms for experimenting with parallel computing, message-passing architectures and historical computer hardware.

One problem when building or restoring such systems is the availability of suitable Transputer processors and associated interface components.

At the same time, inexpensive modern FPGA modules such as the **Tang Nano 20K** provide enough resources to implement a Transputer together with additional peripheral logic in a very small form factor.

The idea behind **Transputer-TRAM-TangNano** is therefore simple:

> Put a modern FPGA implementation of a Transputer into the mechanical and electrical environment of a classic TRAM.

This makes it possible to use the module together with existing Transputer hardware without requiring another custom host interface.

## FPGA Transputer

The Tang Nano 20K carries the FPGA implementation of the Transputer.

The T425 is representative of the widely used 32-bit integer Transputers, while the T805 adds floating-point capabilities and was one of the most powerful members of the classic Transputer family.

The FPGA implementation used by this project is based on joint work by **Andre** , **Claus (cpm)** and **me (dg1vs)**.

### FPGA source code

A ready-to-program **Gowin `.fs` bitstream file** is available.

The FPGA HDL source code itself is currently **not published** as part of this repository.

This means that the supplied FPGA image can be used to operate the hardware, but rebuilding or modifying the FPGA design requires access to the original FPGA sources.

## TRAM Interface

The module follows the basic interface concept of a classic Transputer TRAM.

The hardware provides the four standard Transputer communication links:

- Link 0
- Link 1
- Link 2
- Link 3

Each link consists of a `LinkIn` and `LinkOut` signal.

The TRAM interface also provides the normal Transputer control signals including:

- `ClockIn`
- `Analyse`
- `Reset`
- `notError`
- Link-speed is set fixed to 20 Mbps

This allows the FPGA-based Transputer to behave like a conventional Transputer module from the point of view of the host system.

## 5 V / 3.3 V Level Adaptation

One of the less obvious challenges when combining original Transputer hardware with a modern FPGA is the difference in logic levels.

Classic TRAM and Transputer hardware operates in a **5 V environment**, while the Tang Nano 20K and its FPGA I/O use **3.3 V logic**.

Considerable attention was therefore given to the interface between these two voltage domains.

The current hardware design uses dedicated level-translation circuitry for the Transputer links. Four **TXU0202 dual-channel level translators** provide the eight signal paths required for the four `LinkIn` / `LinkOut` pairs.

Additional circuitry is used for the Transputer control, clock and status signals.

The intention is to make the module compatible with real Transputer hardware while protecting the modern FPGA from inappropriate signal levels.

## Breakout Connectors

Several signals are additionally available on 2.54 mm pin headers.

These connectors are useful for:

- Sound interface (quite cool for an transputer)
- debugging
- logic-analyzer measurements
- FPGA experiments
- connecting additional peripherals
- accessing selected Transputer signals
- experimenting with Transputer links

The PCB contains dedicated connectors for link, event and audio-related signals.

This makes the board useful not only as a replacement TRAM but also as a development platform for Transputer experiments.

## Audio / I²S

Audio output is supported using the **I²S/PCM facilities of the Tang Nano 20K**.

The Tang Nano 20K contains onboard audio hardware, and the TRAM carrier additionally provides an audio connector for access to the relevant signals.

This allows Transputer or FPGA applications to generate sound without requiring a separate audio subsystem.

## ATW800/2

The module can be used with the **ATW800/2** Atari Transputer system.

The ATW800/2 provides conventional size-1 TRAM slots for physical Transputer modules, making the Transputer-TRAM-TangNano a natural companion for this system.

## Hardware

The PCB was designed with **KiCad**.

The main project files are located in:

```text
hardware/tram_tangnano/
    tram_tangnano.kicad_pro
    tram_tangnano.kicad_sch
    tram_tangnano.kicad_pcb
```

The project uses the separate **Transputer-Kicad-Library** as a Git submodule:

```text
hardware/libs/Transputer-Kicad-Library
```

After cloning the repository, initialize the submodule with:

```bash
git submodule update --init --recursive
```

## FPGA Programming

The supplied `.fs` file is a ready-to-program Gowin FPGA bitstream for the Tang Nano 20K.

It can be programmed using the usual Tang Nano 20K programming tools, for example:

- Gowin Programmer
- openFPGALoader

The FPGA source project is currently not included in this repository.

## Project Status

This is an experimental retro-computing hardware project.

The board has already gone through several hardware revisions, particularly around the 5 V / 3.3 V interface.

See `CHANGELOG.md` for known issues and changes between PCB revisions.

When building a board, always check the current schematic, PCB revision and changelog before ordering PCBs or assembling components.

## Credits

- **Andre**
- **Claus (cpm)**


## References

A useful technical background reference for implementing the Transputer architecture in an FPGA is:

[1] Uwe, Mielke & Martin, Zabel & Michael, Bruestle. (2019). T42 – Transputer in FPGA. 10.3233/978-1-61499-949-2-525.  



