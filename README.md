# ♟️ Python Chess Engine

> A fully playable local chess game built from scratch in Python — move generation, legal filtering, special rules, and a Tkinter GUI.


<!-- PHOTO 1: Add a screenshot of the GUI board here -->
<!-- ![Chess Board Screenshot](path/to/screenshot.png) -->

---

## 🚀 Quick Start

### 🪟 Windows — No Python needed

Grab the prebuilt exe from [Releases](../../releases) and just run it. No install, no setup.

```
dist/main.exe  ← double click and go
```

### 🐍 Run from source

```bash
python main.py
```

That's it. The board opens. Click and play.

### 📦 Build the exe yourself

```bash
pyinstaller --onefile --windowed main.py
```

Output lands in `dist/main.exe`.

---

## 🎮 Controls

| Action | How |
|---|---|
| Select a piece | Click it |
| Move | Click a highlighted square |
| Switch selection | Click another friendly piece |
| Promote a pawn | Pick from the popup |
| Step back in history | `←` Left Arrow |
| Step forward | `→` Right Arrow |

---

## 📁 Project Structure

```
Python-chess-engine/
├── main.py                  # Entry point
├── engine/
│   ├── board.py             # Board state, make/undo move, castling & en passant state
│   └── move_generator.py    # Move gen, attack detection, check, legal filtering
├── ui/
│   └── gui.py               # Tkinter board + player interaction
└── tests/
    ├── test_castling.py
    ├── test_game_state.py
    └── test_pawn_promotion.py
```

---

## ⚙️ How The Engine Works

### Board State
`engine/board.py` tracks everything needed to reconstruct any position:
- `board` — the 8×8 grid
- `turn` — whose move it is
- `castling_rights` — which sides still have castling available
- `en_passant_target` — set after any two-square pawn push
- `halfmove_clock` — tracks progress toward the 50-move rule

### Move Generation Flow
```
get_piece_moves()        → pseudo-legal moves per piece type
    ↓
get_legal_moves()        → simulate each move, discard self-checks
    ↓
is_square_attacked()     → used for check + castling validation
    ↓
get_game_status()        → ongoing / check / checkmate / stalemate / draw
```

### Reversible Moves
Every move is tested safely using `make_move()` / `undo_move()`. These handle captures, en passant, castling rook placement, promotion, and full state restoration — so legal filtering never corrupts the board.

---

## ✅ Features

### Pieces
All six piece types — pawns, knights, bishops, rooks, queens, kings.

### Special Rules
- **Castling** — both kingside and queenside, with full legality checks (no moving through check, rights tracking, rook presence)
- **En Passant** — target square tracked per move, captured pawn removed and restored on undo
- **Pawn Promotion** — defaults to queen; GUI shows a popup for human moves

### Check & Endgame Detection
- Check highlighting (king square turns red)
- Illegal move feedback (tried a pinned piece move? king flashes red)
- Checkmate
- Stalemate
- Insufficient material (K vs K, K+B vs K, K+N vs K, and same-color bishop endgames)
- 50-move rule (auto-enforced at halfmove clock = 100)

### GUI Extras
- Selected square highlighted
- Legal destinations outlined
- Status bar: whose turn / game result
- Arrow-key move history navigation
- Promotion popup with piece choice


<!-- PHOTO 2: Add a screenshot of the promotion popup or check highlight here -->
<!-- ![Promotion Popup Screenshot](path/to/promotion.png) -->

---

## 🧪 Tests

```bash
# Run all tests
py -m unittest tests\test_castling.py tests\test_pawn_promotion.py

# Syntax check only (no .pyc files)
$env:PYTHONDONTWRITEBYTECODE='1'; py -m py_compile main.py engine\board.py engine\move_generator.py ui\gui.py tests\test_castling.py tests\test_pawn_promotion.py
```

**What's covered:**
- Queenside castling legality + rook placement
- Undo after castling
- White/black default queen promotion
- Custom knight and rook promotions
- Undo after promotion
- Checkmate, stalemate, insufficient material
- 50-move rule + halfmove clock behavior

---

## 🔧 Known Limitations

This is a solid, playable engine — not a full production system yet.

- No threefold repetition detection
- No AI opponent / search
- No move history export (PGN/notation)
- No timers or clocks
- Unicode piece symbols may render oddly on some systems (needs cleanup in `gui.py`)

---

## 🗺️ What's Next

- [ ] Threefold repetition detection
- [ ] Fix Unicode chess symbols
- [ ] Move history panel in the GUI
- [ ] PGN export
- [ ] AI opponent (minimax + alpha-beta pruning)
- [ ] En passant + pinned piece test coverage

---

## 🏗️ Built With

- **Python 3**
- **Tkinter** — GUI
- **unittest** — testing
- **PyInstaller** — Windows exe packaging

---

*A clean base to keep building on.*
