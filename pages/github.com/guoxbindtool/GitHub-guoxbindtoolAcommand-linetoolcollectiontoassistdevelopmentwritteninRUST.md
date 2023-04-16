---
title: GitHub - guoxbin/dtool: A command-line tool collection to assist development written in RUST
pageTitle: GitHub - guoxbin/dtool: A command-line tool collection to assist development written in RUST
length: 2455
author: guoxbin
timestamp: 2023-04-16T15:03:02-05:00
markdownload-tags: []
markdownload-source: https://github.com/guoxbin/dtool
markdownload-hostname: github.com
---

# GitHub - guoxbin/dtool: A command-line tool collection to assist development written in RUST

## Excerpt
> A command-line tool collection to assist development written in RUST - GitHub - guoxbin/dtool: A command-line tool collection to assist development written in RUST

---
## dtool

[![Build Status][fig1]](https://travis-ci.org/guoxbin/dtool) [![Crates.io][fig2]](https://crates.io/crates/dtool)

`dtool` is a command-line tool collection to assist development

## Table of Contents

-   [Description](https://github.com/guoxbin/dtool#description)
-   [Usage](https://github.com/guoxbin/dtool#usage)
    -   [Tips](https://github.com/guoxbin/dtool#tips)
-   [Installation](https://github.com/guoxbin/dtool#installation)

## Description

Now `dtool` supports:

-   [Hex / UTF-8 string / binary / byte array conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#hex--utf-8-string--binary--byte-array-conversion)
-   [Timestamp / date conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#timestamp--date-conversion)
-   [Number 10/2/8/16 base conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#number-102816-base-conversion)
-   [Hex / base58 conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#hex--base58-conversion)
-   [Hex / base64 conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#hex--base64-conversion)
-   [URL encode / decode](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#url-encode--decode)
-   [Number codec](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#number-codec)
-   [Hash (MD5, SHA-1, SHA-2, SHA-3, RIPEMD, CRC, Blake2b, SM3, Twox)](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#hash-md5-sha-1-sha-2-sha-3-ripemd-crc-blake2b-sm3-twox)
-   [UTF-8 string / unicode conversion](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#utf-8-string--unicode-conversion)
-   [HTML entity encode / decode](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#html-entity-encode--decode)
-   [Regex match](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#regex-match)
-   [Pbkdf2](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#pbkdf2)
-   [Case conversion (upper, lower, title, camel, pascal, snake, shouty snake, kebab, sarcasm)](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#case-conversion-upper-lower-title-camel-pascal-snake-shouty-snake-kebab-sarcasm)
-   [AES encrypt / decrypt](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#aes-encrypt--decrypt)
-   [ECDSA (Secp256k1, NIST P-256, NIST P-384, SM2)](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#ecdsa-secp256k1-nist-p-256-nist-p-384-sm2)
-   [SM4 encrypt / decrypt](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#sm4-encrypt--decrypt)
-   [EdDSA (Ed25519)](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#eddsa-ed25519)
-   [sr25519 signature](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md#sr25519-signature)

## Usage

`dtool` does different works by different sub commands:

| Sub command | Desc | Example |
| --- | --- | --- |
| h2s | Convert hex to UTF-8 string  
v0.1.0 | $ dtool h2s 0x61626364  
abcd |
| s2h | Convert UTF-8 string to hex  
v0.1.0 | $ dtool s2h abcd  
0x61626364 |
| h2b | Convert hex to binary  
v0.1.0 | $ dtool h2b 0x61626364  
abcd |
| b2h | Convert binary to hex  
v0.1.0 | $ dtool b2h abcd  
0x61626364 |
| ... |  |  |

[View full usage document](https://github.com/guoxbin/dtool/blob/master/docs/Usage.md)

-   Besides the sub command `help`, `dtool` provides a sub command `usage` to show examples:

```shell
$ dtool usage
Usage
----------------------------------------------------------
 h2s  Convert hex to UTF-8 string  $ dtool h2s 0x61626364 
      v0.1.0                       abcd 
----------------------------------------------------------
...
```

-   You can search usage with a keyword:

```shell
$ dtool usage -s md5
Usage
-------------------------------------------------------
 hash  Hex to hash  $ dtool hash -a md5 0x616263 
       MD5          0x900150983cd24fb0d6963f7d28e17f72 
       v0.2.0        
-------------------------------------------------------
```

## Tips

### pipe

convert a string to base64

```
$ echo -n abc | dtool s2h | dtool h2b64
YWJj
```

convert a encoded timestamp to date

```
$ echo -n 2c28e75d | dtool nd -tu32 | dtool ts2d
2019-12-04 11:29:48
```

convert a jpeg to base64

```
$ cat pic.jpg | dtool b2h | dtool h2b64
/9j/4AAQSkZJR...
```

calculate file md5

```
$ cat pic.jpg | dtool b2h | dtool hash -a md5
0x1884b72e23b0c93320bac6b050478ff4
```

## Installation

### Homebrew

```shell
$ brew install guoxbin/guoxbin/dtool
```

Recommend! Homebrew will install shell completion for bash, fish, and zsh along with `dtool`

### Arch Linux

There is [an AUR package for dtool](https://aur.archlinux.org/packages/dtool/) that includes shell completion for bash, fish, and zsh.

```shell
git clone https://aur.archlinux.org/dtool.git
cd dtool
makepkg -si
```

### Cargo

[fig1]: https://camo.githubusercontent.com/bebff2109c775d7149bffdb5c86f5e48ae48983ca7a551f9b70df2303a1d7afa/68747470733a2f2f7472617669732d63692e6f72672f67756f7862696e2f64746f6f6c2e7376673f6272616e63683d6d6173746572
[fig2]: https://camo.githubusercontent.com/9667b4117b69ddecd77c674ee0dc2d483fec41c36fecc2df893d3fa8558cf1fc/68747470733a2f2f696d672e736869656c64732e696f2f6372617465732f762f64746f6f6c

> Saved from https://github.com/guoxbin/dtool on 2023-04-16T15:03:02-05:00