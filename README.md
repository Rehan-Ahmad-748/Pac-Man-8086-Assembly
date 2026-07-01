# Pac-Man — 8086 Assembly

A fully playable Pac-Man game written entirely in 8086 Assembly language. No game engine, no high-level language — everything from ghost AI to sound effects is built from scratch at the instruction level.

---

## What It Does

- Multi-level gameplay with increasing difficulty
- Ghost AI with movement logic and collision detection
- Power-ups, lives system, and score tracking
- Sound effects and screen rendering at 640×480
- High score system saved to a file
- Full menu navigation with pause/resume support

---

## Tech Stack

`8086 Assembly` `Irvine Library` `Visual Studio`

---

## Project Structure

```
PACMAN/
└── pacman.asm    → full assembly source code
```

---

## How to Run

1. Open the project in **Visual Studio** with the Irvine Library configured
2. Assemble `pacman.asm` using MASM
3. Run the executable — use keyboard arrows to play

---

## Notes

- Requires the **Irvine32 library** to be set up in Visual Studio
- Built and tested on Windows with MASM assembler
- One of the most complex projects in the degree — every single feature is implemented manually in Assembly

---

*Built as part of BS Computer Science coursework at FAST-NUCES Islamabad.*
