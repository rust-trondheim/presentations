# Agenda rust meetup

1. What is WebAssembly (WASM)
2. What is WebAssembly System Interface (WASI)
3. What is WebSys
4. How to make a rust thing that runs in the browser
5. Demo of an application (look at dev console too)
6. Look at the code
   1. Cargo.toml
   2. WeeAlloc


---

WebAssembly (abv. Wasm) is a binary instruction format for stack-based virtual machines.

_(For us rustaceans that means it is another compilation target.)_

---

- Portable
- Executes at native speed
- Safe by default (sandboxed, memory safe etc)
- Open and debuggable
- Part of the open web platform

---

Why Rust and WASM?

- Rust has no runtime and garbage collector = small bundle size
- Rust is more performant than JavaScript
- Rust has a type system
- You can port your codebase piece by piece (Supports npm, webpack, ECMAScript modules)
- Rust is good and cool 😎

---

Side quest: What is WebAssembly System Interface (WASI)

---

WASM is an assembly language for a conceptual machine. WASI is an interface for a conceptual operating system.

Run your compiled code across all different OSs.

---

More and more tools want to run WASM outside of the browser.

On the edge, in kubernetes, or directly on user machines.

---

Back on track: WASM in the browser

Three important tools

- wasm-bindgen
- wasm-websys
- js-sys

---

wasm-bindgen

- Rust library and CLI
- Import JavaScript functionality to Rust such as classes, functions, etc.
- Work with JS types like strings, numbers, classes, closures and objects
- Automatically generate TypeScript bindings for Rust code being consumed by JS
- Publish your Rust library as an NPM package with full TypeScript support with wasm-pack

---

web-sys

The web-sys crate provides raw wasm-bindgen imports for all of the Web's APIs. This includes:

- window.fetch
- Node.prototype.appendChild
- WebGL
- WebAudio
- and many more!
- It's sort of like the libc crate, but for the Web.

---

js-sys

JavaScript APIs that are guaranteed to exist in all standards-compliant ECMAScript environments, such as Array, Date, and eval.

---

Code: wasm-bindgen in practice

---

Higher level libraries for DOM interaction

---

# yew.rs

```rust
use yew::prelude::*;

#[function_component]
fn App() -> Html {
    let counter = use_state(|| 0);
    let onclick = {
        let counter = counter.clone();
        move |_| {
            let value = *counter + 1;
            counter.set(value);
        }
    };

    html! {
        <div>
            <button {onclick}>{ "+1" }</button>
            <p>{ *counter }</p>
        </div>
    }
}

fn main() {
    yew::Renderer::<App>::new().render();
}
```

---

# Leptos.dev

```rust
#[component]
fn App(cx: Scope) -> impl IntoView {
    let (count, set_count) = create_signal(cx, 0);

    view! { cx,
        <button
            on:click=move |_| {
                set_count(3);
            }
        >
            "Click me: "
            {move || count.get()}
        </button>
    }
}

fn main() {
    leptos::mount_to_body(|cx| view! { cx, <App/> })
}

```


___

Demo of a real app

---

Resources:
https://developer.mozilla.org/en-US/docs/WebAssembly/Rust_to_wasm#building_our_webassembly_package
https://github.com/rustwasm/wee_alloc
https://rustwasm.github.io/docs/wasm-bindgen/
https://rustwasm.github.io/book/game-of-life/hello-world.html
https://webassembly.org/

Frameworks: 
https://leptos.dev
https://yew.rs/
https://dioxuslabs.com/
