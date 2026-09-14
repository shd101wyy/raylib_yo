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
{ InitWindow, CloseWindow, BeginDrawing, EndDrawing, ClearBackground, Color } :: import("raylib_yo");
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

```rust
{ InitWindow, CloseWindow, WindowShouldClose, SetTargetFPS,
  BeginDrawing, EndDrawing, ClearBackground, DrawText,
  RAYWHITE, DARKGRAY } :: import("raylib_yo");

// raylib's functions are extern "c", so every call site has to be
// audit-capable. `pragma` grants the file the privilege; `unsafe(...)` marks
// the individual calls.
pragma(Pragma.AllowUnsafe);

main :: (fn() -> unit)({
  unsafe(InitWindow(i32(800), i32(450), "Hello Raylib from Yo!"));
  unsafe(SetTargetFPS(i32(60)));

  while(!unsafe(WindowShouldClose()), {
    unsafe(BeginDrawing());
    unsafe(ClearBackground(RAYWHITE));
    unsafe(DrawText("Hello, World!", i32(190), i32(200), i32(20), DARKGRAY));
    unsafe(EndDrawing());
  });

  unsafe(CloseWindow());
});

export(main);
```

This is `src/main.yo` in this repository, give or take the window title — it is
built and run by `yo build run`, so it cannot drift from what compiles.

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
