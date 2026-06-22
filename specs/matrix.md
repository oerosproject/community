Support matrix (high level)
============================

ROS Distros
- Jazzy (LTS) — Top priority
- Lyrical (LTS) — Top priority
- Humble
- Kilted
- Rolling

Yocto Releases
- Scarthgap (LTS) — Top priority
- Wrynose (LTS) — Top priority
- Whinlatter

BSPs / Boards
- Raspberry Pi 4
- Raspberry Pi 5
- Nvidia Orin Nano
- Nvidia Orin AGX
- AMD Kria KR260
- Qualcomm RB3
- Microchip PolarFire

Architectures
- aarch64 (primary)
- armv7
- x86_64
- riscv

Notes
- Prioritize building and CI test coverage for the intersection of (Jazzy/Lyrical) x (Scarthgap/Wrynose) x (RPI4/RPI5/Orin/AGX/Kria/RB3/PolarFire).
- Track gaps as specs with clear owners.
