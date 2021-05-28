# Ketchup Maze

Ketchup Maze este o extensie a bootloader-ului UEFI [TomatBoot](https://github.com/TomatOrg/TomatBoot), ce implementeaza un joc de tip labirint.
Proiectul este scris in C, cu ajutorul mediului de dezvoltare [EDK2](https://github.com/tianocore/edk2).

# Preview

Meniu principal         |  Alegere nivel
:-------------------------:|:-------------------------:
![](/screenshots/main_menu.jpeg)  |  ![](/screenshots/level_menu.jpeg)

Culori         |  Gameplay
:-------------------------:|:-------------------------:
![](/screenshots/green.jpeg)  |  ![](/screenshots/win.jpeg)

## Instructiuni de utilizare

Proiectul a fost dezvoltat, compilat si testat pe Linux.

### Dependente
Pentru a rula proiectul, este nevoie de:
- [QEMU](https://www.qemu.org/)
- [EDK2](https://github.com/tianocore/edk2)
- Compilator C
- Compilator NASM
```
sudo apt-get install build-essential qemu nasm ovmf
```
Pentru a se pregati mediul de dezvoltare EDK2, se vor urma instructiunile de pe site-ul lor.<br>
Recomand urmatoarea [metoda / tutorial](https://wiki.ubuntu.com/UEFI/EDK2).

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

### Element de inovatie
Proiectul curent aduce, in plus, partea de integrare intr-un bootloader sursa, capabil sa incarce un SO. <br>
Astfel, se poate utiliza in viitor chiar si in afara unei masini virtuale (ex.: QEMU), ca bootloader principal al unui sistem de calcul.

## Detalii de implementare

### Contributii personale
Urmatoarele fisiere sunt contributia mea personala, integral:
- src/menus/ChooseLevel.c
- src/menus/GameUtils.c
- src/menus/EasyGameMenu.c
- src/menus/MediumGameMenu.c
- src/menus/HardGameMenu.c
- src/util/MemUtils.c

Partial:
- src/menus/Menus.h
- src/menus/Menus.c
- src/menus/MainMenu.c

### Descreire implementare functii
Initial, am cautat sa inteleg proiectul sursa, pentru a integra cat mai bine jocul in acesta. Necesitatea functiilor a fost dedusa treptat, observand portiunile de cod ce se repeta / sunt foarte complexe in scriere. <br>
Majoritatea functiilor utilizate se afla in src/menus/GameUtils.c, iar implementarile principale sunt:

#### Desenarea labirintului - DrawMaze()
Urmeaza modelul desnearii logo-ului bootloaderului sursa. <br>
Astfel, se defineste un array de tip CHAR8 pentru fiecare maze de desenat. Se foloseste CHAR8 deoarece culorile UEFI sunt definite astfel (prin etichete, de ex.: EFI_RED, EFI_GREEN). <br>
Se definesc dimensiunile array-ului (2 dimensiuni - se va accesa ca o matrice), si coordonatele coltului din stanga sus, de unde se va incepe desenarea. Se foloseste wrapperul proiectului sursa: DrawImage(...), ce foloseste UEFI Graphics Output Protocol.

#### Schimbarea culorii - DrawMazeColor(CHAR8 maze_color)
Se definesc array-uri de tip CHAR8 diferite pentru fiecare culoare + un array pe care se va plimba caracterul. Nu este o metoda eficienta, insa este singura gasita momentan, datorita macro-urilor necesare (#define). Functia primeste culoarea dorita si deseneaza pe ecran maze-ul dorit ca si culoare, copiind de fiecare data maze-ul in care se va deplasa caracterul.

#### Deplasarea caracterului - int moveInMaze(int pos, int height, int moves[4])
Functia foloseste UEFI Standard Input Text Protocol, si defineste mutarile posibile intr-o anumita pozitie, facand legatura cu desenarea in maze a acestora.<br>
Functia primeste pozitia pentru care se definesc mutarile (pos), inaltimea maze-ului (height), si un array in care se specifica pozitiile (moves[4]). Ordinea tastelor este W, A, S, D (sus, stanga, jos, dreapta), iar pozitiile posibile sunt marcate cu 1, cele imposibile cu 0. Un exemplu de array: [1, 0, 0, 0], inseamna ca singura mutare valida este in sus (tasta W).

### Probleme intampinate

#### Performantele video
Jocul efectueaza o multime de operatii cu Graphics Output Protocol, solictiand memoria video. La fiecare mutare valida in maze, imaginea acestuia se va reincarca, la fel si in cazul schimbarii culorii, de exemplu. <br>
Incercarea de a implementa labirinturi mai complexe va solicita memoria video, suficient incat sa produca un deranj vizual: timpul de incarcare a liniilor va creste iar "refresh-ul" labirintului se face foarte lent.<br><br>
Rezolvarea problemei ar implica regandirea intregii implementari a mutarilor in maze, insa este luata in considerare pe viitor pentru progresul proiectului. 

#### Warning-uri inutile
La compilare, apar warning-uri de tipul -W-unused-function, deoarece functiile utile nu sunt utilizate in acelasi fisier, ci in alte fisiere.<br><br>
Pentru o compilare "curata", s-au adaugat pragma-uri in aceste cazuri.

