# nanoMODBUS — NuttX Port

Build integration files for running nanoMODBUS on Apache NuttX RTOS.

## What's here

- `Kconfig` — menuconfig entries for enabling nanoMODBUS and selecting
  individual function codes (server/client, FC01-FC43)
- `Makefile` — NuttX make-based build
- `Make.defs` — registers nanoMODBUS as a configured app
- `CMakeLists.txt` — NuttX cmake-based build (alternative to make)

These files map NuttX Kconfig options to the compile-time flags that
nanoMODBUS uses to enable/disable features (e.g. `NMBS_SERVER_DISABLED`,
`NMBS_SERVER_READ_COILS_DISABLED`, etc).

## How to integrate

1. Place (or symlink) the nanoMODBUS repository under your NuttX apps tree:

       cd nuttx-apps/
       ln -s /path/to/nanoMODBUS nanomodbus

2. Copy the port files into the nanomodbus root so NuttX can find them:

       cp nanomodbus/ports/nuttx/Kconfig    nanomodbus/Kconfig
       cp nanomodbus/ports/nuttx/Makefile    nanomodbus/Makefile
       cp nanomodbus/ports/nuttx/Make.defs   nanomodbus/Make.defs
       cp nanomodbus/ports/nuttx/CMakeLists.txt nanomodbus/CMakeLists.txt

   Or symlink them if you prefer:

       cd nanomodbus/
       ln -sf ports/nuttx/Kconfig .
       ln -sf ports/nuttx/Makefile .
       ln -sf ports/nuttx/Make.defs .
       ln -sf ports/nuttx/CMakeLists.txt .

3. Enable in menuconfig:

       make menuconfig
       # -> nanoMODBUS -> Modbus support using nanoMODBUS [y]
       # -> select server/client and function codes as needed

4. Build:

       make

The library compiles `nanomodbus.c` from the repo root. No source
modifications needed — all configuration is done through compile flags.

## Kconfig options

| Option | Default | Description |
|--------|---------|-------------|
| `NANOMODBUS` | n | Master enable |
| `NANOMODBUS_SERVER` | y | Server (slave) support |
| `NANOMODBUS_CLIENT` | y | Client (master) support |
| `NANOMODBUS_STRERROR` | n | Human-readable error strings (~1KB) |
| `NANOMODBUS_SERVER_READ_COILS` | y | FC01 |
| `NANOMODBUS_SERVER_READ_DISCRETE_INPUTS` | y | FC02 |
| `NANOMODBUS_SERVER_READ_HOLDING_REGISTERS` | y | FC03 |
| `NANOMODBUS_SERVER_READ_INPUT_REGISTERS` | y | FC04 |
| `NANOMODBUS_SERVER_WRITE_SINGLE_COIL` | y | FC05 |
| `NANOMODBUS_SERVER_WRITE_SINGLE_REGISTER` | y | FC06 |
| `NANOMODBUS_SERVER_WRITE_MULTIPLE_COILS` | y | FC15 |
| `NANOMODBUS_SERVER_WRITE_MULTIPLE_REGISTERS` | y | FC16 |
| `NANOMODBUS_SERVER_READ_WRITE_REGISTERS` | n | FC23 |
| `NANOMODBUS_SERVER_READ_FILE_RECORD` | n | FC20 |
| `NANOMODBUS_SERVER_WRITE_FILE_RECORD` | n | FC21 |
| `NANOMODBUS_SERVER_READ_DEVICE_IDENTIFICATION` | n | FC43/14 |

## Tested on

- NuttX 12.x on STM32H5 (Cortex-M33)
- nanoMODBUS v1.23.0
- Both make and cmake build systems
