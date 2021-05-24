# Ketchup Maze

Ketchup Maze este o extensie a bootloader-ului UEFI [TomatBoot](https://github.com/TomatOrg/TomatBoot), ce implementeaza un joc de tip labirint.

## Instructiuni de utilizare

Proiectul a fost dezvoltat, compilat si testat pe Linux.

### Dependente
Pentru a rula proiectul, este nevoie de:
- [QEMU](https://www.qemu.org/)
- [EDK2](https://github.com/tianocore/edk2)
- Compilator C
- Compilator NASM
```
sudo apt-get install build-essential qemu nasm 
```

### Compilare
Initial se cloneaza proiectul.

```
cd KetchupMaze
make
```

Pentru a sterge sursele generate:
```
make clean
```

### Testare
Pentru a rula proiectul in QEMU, am pus la dispozitie un script de testare:
```
cd bin
./build_and_test.sh
```

Din QEMU, se va apasa ESC, iar apoi:
```
> fs0:
> bootx64
```

Bootloader-ul se va lansa. Extensia proiectului initial se poate observa prin optiunea G (Game).


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
