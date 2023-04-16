---
title: Home ⚡ Zig Programming Language
pageTitle: Home ⚡ Zig Programming Language
length: 2673
author: 
timestamp: 2023-04-16T15:04:01-05:00
markdownload-tags: []
markdownload-source: https://ziglang.org/
markdownload-hostname: ziglang.org
---

# Home ⚡ Zig Programming Language

## Excerpt
> Zig is a general-purpose programming language and toolchain for maintaining robust, optimal and reusable software.

---
Zig is a general-purpose programming language and toolchain for maintaining **robust**, **optimal** and **reusable** software.

## ⚡ A Simple Language

Focus on debugging your application rather than debugging your programming language knowledge.

-   No hidden control flow.
-   No hidden memory allocations.
-   No preprocessor, no macros.

## ⚡ Comptime

A fresh approach to metaprogramming based on compile-time code execution and lazy evaluation.

-   Call any function at compile-time.
-   Manipulate types as values without runtime overhead.
-   Comptime emulates the target architecture.

## ⚡ Maintain it with Zig

Incrementally improve your C/C++/Zig codebase.

-   Use Zig as a zero-dependency, drop-in C/C++ compiler that supports cross-compilation out-of-the-box.
-   Leverage `zig build` to create a consistent development environment across all platforms.
-   Add a Zig compilation unit to C/C++ projects; cross-language LTO is enabled by default.

```
const std = @import("std");
const json = std.json;
const payload =
    \\{
    \\    "vals": {
    \\        "testing": 1,
    \\        "production": 42
    \\    },
    \\    "uptime": 9999
    \\}
;
const Config = struct {
    vals: struct { testing: u8, production: u8 },
    uptime: u64,
};
const config = x: {
    var stream = json.TokenStream.init(payload);
    const res = json.parse(Config, &stream, .{});
    // Assert no error can occur since we are
    // parsing this JSON at comptime!
    break :x res catch unreachable;
};
pub fn main() !void {
    if (config.vals.production > 50) {
        @compileError("only up to 50 supported");
    }
    std.debug.print("up={d}", .{config.uptime});
}
```

## Community

![][fig1]

## Zig Software Foundation

## The ZSF is a 501(c)(3) non-profit corporation.

The Zig Software Foundation is a non-profit corporation founded in 2020 by Andrew Kelley, the creator of Zig, with the goal of supporting the development of the language. Currently, the ZSF is able to offer paid work at competitive rates to a small number of core contributors. We hope to be able to extend this offer to more core contributors in the future.

The Zig Software Foundation is sustained by donations.

## [Learn More](https://ziglang.org/zsf/)

## Sponsors

The following companies are providing direct financial support to the Zig Software foundation.

Thanks to people who [sponsor Zig](https://ziglang.org/zsf/), the project is accountable to the open source community rather than corporate shareholders. In particular, these fine folks sponsor Zig for $200/month or more:

-   [Pex](https://pex.com/)
-   [Oven](https://oven.sh/)
-   [Kirk Scheibelhut](https://scheibo.com/)
-   [drfuchs](https://github.com/drfuchs)
-   [tarasbob](https://github.com/tarasbob)
-   [CoilHQ](https://coil.com/)
-   Stephen Gutekanst
-   Stevie Hryciw
-   Mitchell Hashimoto
-   Paul Harrington
-   Derek Collison
-   Clark Gaebel
-   Karrick McDermott
-   Joran Dirk Greef
-   bfredl
-   Dustin Taylor
-   Tom Manner
-   José M Rico
-   Simon A. Nielsen Knights
-   MikoVerse

This section is updated at the beginning of each month.

[fig1]: https://ziglang.org/ziggy.svg

> Saved from https://ziglang.org/ on 2023-04-16T15:04:01-05:00