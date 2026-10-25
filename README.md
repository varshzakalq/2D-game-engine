# 2D Game Engine

A small 2D game engine built in C++ for grid-based gameplay. It handles rendering, input, and entity movement while keeping game logic in the `game/` layer.

## Features

- Tile-based grid world with bounds and collision checks
- Entity system for players, enemies, obstacles, and collectibles
- Colored and textured entities
- Automatic collision-safe movement through `SetPos`
- Input handling for keyboard and mouse
- Simple render loop for game updates and drawing
- Modular layout separating engine code from game logic

## Project structure

```text
2D-game-engine/
├── game/           # Game logic, entities, grid, gameplay rules
├── engine/         # Engine systems (core, graphics, physics)
├── sokol/          # Sokol headers and support files
├── main.cpp        # Application entry point
├── CMakeLists.txt  # Build config
├── README.md
└── assets/         # Game sprites and textures
```

## Build

Requirements:
- CMake
- C++ compiler (MinGW-w64 on Windows is common)

From the project root:

```bash
cmake -S . -B build -G "MinGW Makefiles"
cmake --build build
```

Run the app:

```bash
./build/My2DGame.exe
```

## How to use

### 1. Create a game

```cpp
Game game(40, 22, 32);
game.init();
```

### 2. Add an entity

Colored entity:

```cpp
colored_entity* player = new colored_entity(
    5, 10,
    TileType::Player,
    50, 220, 80, 255,
    Board
);
entities.push_back(std::unique_ptr<Entity>(player));
```

Textured entity:

```cpp
texture playerTex;
if (!playerTex.loadfromfile("assets/player.bmp")) {
    playerTex.create_default();
}

entities.push_back(std::unique_ptr<Entity>(new textured_entity(
    2, 2,
    TileType::Player,
    playerTex,
    Board
)));
```

### 3. Move an entity

```cpp
player->SetPos(newX, newY, Board);
```

This checks the grid and prevents movement into blocked or out-of-bounds tiles.

### 4. Update and render each frame

```cpp
void Game::update(float dt) {
    // input, AI, gameplay, movement
}

void Game::render(Renderer& renderer) {
    for (auto& entity : entities) {
        entity->render((float)tileSize, renderer);
    }
}
```

## Core classes

- `Grid`: stores tile data and collision state
- `Entity`: base class for all game objects
- `colored_entity`: renders a solid color box
- `textured_entity`: renders a sprite texture
- `Game`: manages the game loop and entity list

## Notes

- Keep gameplay code inside `game/` when possible.
- Use `Board.empty(x, y)` when removing an entity from a tile.
- Sprite files should be saved as BMP and loaded from `assets/`.

This engine is intended to be easy to extend for small grid-based games, prototypes, and learning projects.
