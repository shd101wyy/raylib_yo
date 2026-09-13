# raylib_yo

Comprehensive [raylib](https://www.raylib.com/) bindings for the [Yo](https://github.com/shd101wyy/Yo) programming language.

## What's Included

- **35 struct types** — `Color`, `Vector2`, `Vector3`, `Vector4`, `Matrix`, `Rectangle`, `Image`, `Texture`, `Font`, `Camera2D`, `Camera3D`, `Shader`, `Sound`, `Music`, and more
- **535 function bindings** — Window management, drawing (2D/3D), input (keyboard/mouse/gamepad/touch), textures, text, shapes, splines, collision detection, audio, and more
- **227 constants** — Keyboard keys, mouse buttons, gamepad buttons/axes, config flags, camera modes, blend modes, gesture types, pixel formats, color presets, and more

## Installation

Dependencies live in `yo.toml`, and `yo add` writes the entry for you:

```bash
yo add shd101wyy/raylib_yo
```

That records it in your `yo.toml` and pins the resolved commit in `yo.lock`:

```toml
[dependencies]
raylib_yo = { git = "https://github.com/shd101wyy/raylib_yo", version = "^0.0.7" }
```

Import it by package name — no paths:

```rust
{ init_window, close_window, begin_drawing, end_drawing, clear_background, Color } :: import("raylib_yo");
```

raylib itself is a **system** library, so link it in `build.yo`:

```rust
build :: import("std/build");

raylib :: build.system_library({ name : "raylib" });

exe :: build.executable({ name : "my_app", root : "./src/main.yo" });
exe.link(raylib);
```

Then build:

```bash
yo build
```

`yo build` fetches declared dependencies on its own. Other commands — `yo check`,
`yo test`, `yo compile`, `yo doc` — resolve imports but never fetch, so on a
fresh clone run `yo install` once.

### Prerequisites

- [Yo](https://github.com/shd101wyy/Yo) compiler
- [raylib](https://www.raylib.com/) system library (install via your package manager or use [devenv](https://devenv.sh/))
- A C compiler (clang recommended)
- `pkg-config`

## Usage

```yo
{ InitWindow, CloseWindow, WindowShouldClose, BeginDrawing, EndDrawing,
  ClearBackground, DrawText, SetTargetFPS, RAYWHITE, DARKGRAY } :: import "raylib_yo";

main :: (fn() -> unit) {
  InitWindow(800, 450, "Hello Raylib from Yo!");
  SetTargetFPS(60);

  while !(WindowShouldClose()), {
    BeginDrawing();
    ClearBackground(RAYWHITE);
    DrawText("Hello, World!", 190, 200, 20, DARKGRAY);
    EndDrawing();
  };

  CloseWindow();
};

export main;
```

## API Coverage

| Module | Description |
|--------|-------------|
| **Core** | Window, cursor, drawing modes, shaders, screen/world transforms, timing, frame control, random, file system, automation events |
| **Input** | Keyboard, mouse, gamepad, touch, gestures |
| **Shapes** | Pixels, lines, circles, ellipses, rings, rectangles, triangles, polygons, splines, collision detection |
| **Textures** | Image loading/generation/manipulation/drawing, texture loading/config/drawing, color/pixel functions |
| **Text** | Font loading, text drawing, text measurement, string manipulation |
| **3D** | 3D shape primitives (cubes, spheres, cylinders, capsules, planes), 3D collision detection |
| **Audio** | Audio device, wave/sound loading/playback, music streaming, audio streams |

> **Note:** Complex 3D model types (`Mesh`, `Model`, `Material`) are bound as struct types. Some raylib functions involving callback types or very complex signatures may require `*(void)` casts.

## Development

This project uses [devenv](https://devenv.sh/) for development environment management:

```bash
direnv allow .   # Activate nix shell (one-time)
yo build         # Build the library
yo build run     # Build and run the demo
```

## License

MIT
