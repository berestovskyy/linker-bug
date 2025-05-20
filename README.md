Linker Bug
==========

To reproduce:

```shell
$ cargo build
   Compiling linker-bug v0.1.0 (/home/a/tmp/linker-bug)
error: linking with `cc` failed: exit status: 1
  |
  = note:  "cc" "-Wl,--version-script=/tmp/rustc8hPuFH/list" "-Wl,--no-undefined-version" "-m64" "/tmp/rustc8hPuFH/symbols.o" "<2 object files omitted>" "-Wl,--as-needed" "-Wl,-Bstatic" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib/{libstd-*,libpanic_unwind-*,libobject-*,libmemchr-*,libaddr2line-*,libgimli-*,librustc_demangle-*,libstd_detect-*,libhashbrown-*,librustc_std_workspace_alloc-*,libminiz_oxide-*,libadler2-*,libunwind-*,libcfg_if-*,liblibc-*,liballoc-*,librustc_std_workspace_core-*,libcore-*,libcompiler_builtins-*}.rlib" "-Wl,-Bdynamic" "-lgcc_s" "-lutil" "-lrt" "-lpthread" "-lm" "-ldl" "-lc" "-L" "/tmp/rustc8hPuFH/raw-dylibs" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-L" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-o" "/home/a/tmp/linker-bug/target/debug/deps/liblinker_bug.so" "-Wl,--gc-sections" "-shared" "-Wl,-z,relro,-z,now" "-nodefaultlibs"
  = note: some arguments are omitted. Use `--verbose` to show all linker arguments
  = note: /usr/bin/ld:/tmp/rustc8hPuFH/list:3: syntax error in VERSION script
          collect2: error: ld returned 1 exit status
          

error: could not compile `linker-bug` (lib) due to 1 previous error
```

Removing the `wasm32` target from `rust-toolchain.toml`: same error for `x86_64`:

```shell
$ cargo build
   Compiling linker-bug v0.1.0 (/home/a/tmp/linker-bug)
error: linking with `cc` failed: exit status: 1
  |
  = note:  "cc" "-Wl,--version-script=/tmp/rustcXt30vN/list" "-Wl,--no-undefined-version" "-m64" "/tmp/rustcXt30vN/symbols.o" "<2 object files omitted>" "-Wl,--as-needed" "-Wl,-Bstatic" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib/{libstd-*,libpanic_unwind-*,libobject-*,libmemchr-*,libaddr2line-*,libgimli-*,librustc_demangle-*,libstd_detect-*,libhashbrown-*,librustc_std_workspace_alloc-*,libminiz_oxide-*,libadler2-*,libunwind-*,libcfg_if-*,liblibc-*,liballoc-*,librustc_std_workspace_core-*,libcore-*,libcompiler_builtins-*}.rlib" "-Wl,-Bdynamic" "-lgcc_s" "-lutil" "-lrt" "-lpthread" "-lm" "-ldl" "-lc" "-L" "/tmp/rustcXt30vN/raw-dylibs" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-L" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-o" "/home/a/tmp/linker-bug/target/debug/deps/liblinker_bug.so" "-Wl,--gc-sections" "-shared" "-Wl,-z,relro,-z,now" "-nodefaultlibs"
  = note: some arguments are omitted. use `--verbose` to show all linker arguments
  = note: /usr/bin/ld:/tmp/rustcXt30vN/list:3: syntax error in VERSION script
          collect2: error: ld returned 1 exit status
          

error: could not compile `linker-bug` (lib) due to 1 previous error
```

Removing `rust-toolchain.toml` file: same error.

Workaround 1: Specifying the target on the command line produces no error.

```shell
$ cargo build --target=wasm32-unknown-unknown
   Compiling linker-bug v0.1.0 (/home/a/tmp/linker-bug)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
```
Workaround 2: Removing spaces from the exported name produces no errors.
