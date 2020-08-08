# blog_os
practice for blog_os (https://os.phil-opp.com/): simple OS structuring by RUST

# A Freestanding Rust Bianry

To write an operating system kernel, we need code that does not depend on any operating system features.

- Disable stdlib, libc by: <br>
    ```
    /// in main.rs
    #![no_std]
    ```
- Implement Panic: <br>
    ```
    /// in main.rs
    use::core::panic::PanicInfo
    
    #[panic_handler]
    fn panic(_info: &PanicInfo) -> ! {
        loop {}
    }
    ```
- `!`: `never` type to prevent returning.
- Disable stack unwinding by aborting panic.   
_disabling due to implementation complexity of stack unwinding_
    ```
    /// in Cargo.toml
    [profile.dev]
    panic = "abort"

    [profile.release]
    panic = "abort"
    ```
- Add `#![no_main]` and remove `fn main {}` to overwrite entry point.
- Add _start with no_mangle(for link) attribute which will be new entry point.    
    _To avoid called by other frame,_ `_start` _should return nothing._
    ```
    #[no_mangle]
    pub extern "C" fn _start() -> ! {
        loop {}
    }
    ```
- Resolve linker error: Building for a Bare Metal Target. For example,
    ```
    rustup target add thumbv7em-none-eabihf
    cargo build --target thumbv7em-none-eabihf
    ```