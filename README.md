# Ketchup Maze

Ketchup Maze este o extensie a bootloader-ului UEFI [TomatBoot](https://github.com/TomatOrg/TomatBoot), ce implementeaza un joc de tip labirint.

## Proiecte similare (standalone)
- [The UEFI Maze Game](https://uefi.blogspot.com/2016/11/the-uefi-maze-game-part-1.html)
- [Firmware Maze](https://github.com/liute62/Firmware-UEFI-Maze-Game)

## Detalii de implementare

Proiectul este scris in C, si utilizeaza librarii specifice UEFI, din platforma [EDK2](https://github.com/tianocore/edk2).

### Contributii personale
Urmatoarele fisiere sunt contributia mea personala, integral:
- src/menus/ChooseLevel.c
- src/menus/GameUtils.c
- src/menus/EasyGameMenu.c
- src/menus/MediumGameMenu.c
- src/menus/HardGameMenu.c
- src/util/MemUtils.c

Partial:
- src/util/DrawUtils.c
- src/menus/Menus.h
- src/menus/Menus.c
- src/menus/MainMenu.c
