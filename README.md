# Super90

An emulator for the **SGI IP27** (Origin 200 / Origin 2000), written in Rust.

> **Status: work in progress.** Nothing boots yet. The project is at the very beginning, and the interface and layout will change.

## Goals

- A MIPS64 (R10000-class, MIPS IV) CPU core, including CP0, the TLB and the FPU
- The IP27 node: Hub ASIC, NASID-based physical address map, memory
- XIO / Bridge / PCI, with an IOC3 serial console
- Booting the IP27 PROM, and later an operating system (Linux/MIPS first, IRIX later)
- Multi-node (NUMAlink) support, as a long-term optional goal

## Firmware

**Super90 does not include any firmware, and it never will.** The SGI IP27 PROM is copyrighted software, so it is not part of this repository and is not distributed with it.

To run the emulator you must supply your own IP27 PROM image (`ip27prom.bin`), dumped from hardware you own or obtained in a way that is legal where you live. This project does not provide or link to firmware downloads.

The emulator takes the path to the image at startup:

```
super90 --prom /path/to/ip27prom.bin
```

Planned behavior:

- The path can also come from the `SUPER90_PROM` environment variable.
- The file's size and SHA-256 are checked against a list of known revisions. An unknown image produces a warning, not a crash.
- If no firmware is given, the emulator exits with an error message.

The `.gitignore` excludes `firmware/`, `*.bin`, `*.rom` and `*.prom`. If you contribute, **never commit a firmware image.**

Unit tests do not need firmware. They use small hand-written MIPS programs, so the project builds and tests without any proprietary files.

## Building

You need a recent stable Rust toolchain ([rustup.rs](https://rustup.rs)).

```
cargo build --release
cargo test
```

## Planned layout

```
Super90/
├── Cargo.toml          workspace
├── crates/
│   ├── mips/           CPU core: decode, execute, CP0, TLB, FPU
│   ├── ip27/           Hub, memory map, node, Bridge, IOC3
│   └── super90/        binary: CLI, config, firmware loading
└── tests/              instruction tests and golden traces
```

## Roadmap

1. MIPS64 integer interpreter
2. CP0, TLB and exceptions
3. FPU (CP1)
4. Single-node Hub and memory map
5. Bridge + IOC3 UART (serial console)
6. SCSI and Ethernet
7. Multi-node / NUMAlink

## Legal

Super90 is an independent project. It is not affiliated with or endorsed by Silicon Graphics International Corp., Hewlett Packard Enterprise, or any other rights holder. SGI, IRIX, Origin and related names are trademarks of their respective owners.

## License

MIT. See [LICENSE](LICENSE).
