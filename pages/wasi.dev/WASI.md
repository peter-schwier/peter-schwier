---
title: WASI |
pageTitle: WASI |
length: 646
author: 
timestamp: 2023-04-16T15:02:11-05:00
markdownload-tags: []
markdownload-source: https://wasi.dev/
markdownload-hostname: wasi.dev
---

# WASI |

## Excerpt
> WASI is a modular system interface for WebAssembly. As described in the initial announcement, it’s focused on security and portability.

---
## The WebAssembly System Interface

WASI is a modular system interface for WebAssembly. As described in [the initial announcement](https://hacks.mozilla.org/2019/03/standardizing-wasi-a-webassembly-system-interface/), it’s focused on security and portability.

WASI is being standardized in [a subgroup of the WebAssembly CG](https://github.com/WebAssembly/WASI/blob/main/Charter.md). Discussions happen in [GitHub issues](https://github.com/WebAssembly/WASI/issues), [pull requests](https://github.com/WebAssembly/WASI/pulls), and [bi-weekly Zoom meetings](https://github.com/WebAssembly/meetings/tree/main/wasi).

For a quick intro to WASI, including getting started using it, see [the intro document](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-intro.md).

The Wasmtime runtime’s [tutorial](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-tutorial.md) contains [examples](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-tutorial.md#running-common-languages-with-wasi) for how to target WASI from [C](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-tutorial.md#from-c) and [Rust](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-tutorial.md#from-rust). The resulting .wasm modules can be run in any WASI-compliant runtime.

For more documentation, see [the documents guide](https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-documents.md).

> Saved from https://wasi.dev/ on 2023-04-16T15:02:11-05:00