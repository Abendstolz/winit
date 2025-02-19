# winit - Cross-platform window creation and management in Rust

[![](http://meritbadge.herokuapp.com/winit)](https://crates.io/crates/winit)
[![Docs.rs](https://docs.rs/winit/badge.svg)](https://docs.rs/winit)
[![Build Status](https://travis-ci.org/rust-windowing/winit.svg?branch=master)](https://travis-ci.org/rust-windowing/winit)
[![Build status](https://ci.appveyor.com/api/projects/status/hr89but4x1n3dphq/branch/master?svg=true)](https://ci.appveyor.com/project/Osspial/winit/branch/master)

```toml
[dependencies]
winit = "0.20.0-alpha1"
```

## [Documentation](https://docs.rs/winit)

## Contact Us

Join us in any of these:

[![Freenode](https://img.shields.io/badge/freenode.net-%23glutin-red.svg)](http://webchat.freenode.net?channels=%23glutin&uio=MTY9dHJ1ZSYyPXRydWUmND10cnVlJjExPTE4NSYxMj10cnVlJjE1PXRydWU7a)
[![Matrix](https://img.shields.io/badge/Matrix-%23Glutin%3Amatrix.org-blueviolet.svg)](https://matrix.to/#/#Glutin:matrix.org)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tomaka/glutin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## Usage

Winit is a window creation and management library. It can create windows and lets you handle
events (for example: the window being resized, a key being pressed, a mouse movement, etc.)
produced by window.

Winit is designed to be a low-level brick in a hierarchy of libraries. Consequently, in order to
show something on the window you need to use the platform-specific getters provided by winit, or
another library.

```rust
use winit::{
    event::{Event, WindowEvent},
    event_loop::{ControlFlow, EventLoop},
    window::WindowBuilder,
};

fn main() {
    let event_loop = EventLoop::new();
    let window = WindowBuilder::new().build(&event_loop).unwrap();

    event_loop.run(move |event, _, control_flow| {
        match event {
            Event::WindowEvent {
                event: WindowEvent::CloseRequested,
                window_id,
            } if window_id == window.id() => *control_flow = ControlFlow::Exit,
            _ => *control_flow = ControlFlow::Wait,
        }
    });
}
```

Winit is only officially supported on the latest stable version of the Rust compiler.

### Cargo Features

Winit provides the following features, which can be enabled in your `Cargo.toml` file:
* `serde`: Enables serialization/deserialization of certain types with [Serde](https://crates.io/crates/serde).

### Platform-specific usage

#### Emscripten and WebAssembly

Building a binary will yield a `.js` file. In order to use it in an HTML file, you need to:

- Put a `<canvas id="my_id"></canvas>` element somewhere. A canvas corresponds to a winit "window".
- Write a Javascript code that creates a global variable named `Module`. Set `Module.canvas` to
  the element of the `<canvas>` element (in the example you would retrieve it via `document.getElementById("my_id")`).
  More information [here](https://kripken.github.io/emscripten-site/docs/api_reference/module.html).
- Make sure that you insert the `.js` file generated by Rust after the `Module` variable is created.
