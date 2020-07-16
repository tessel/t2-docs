# Supported languages

* [Javascript/Node.js](#javascriptnodejs)
  * [Binary Modules](#binary-modules)
* [Rust](#rust)
* [Other Languages](#other-languages)

## JavaScript/Node.js
Tessel 2 has full support for JavaScript and Node.js (LTS versions). The relevant repos can be found here:

* [Firmware support for JavaScript/Node](https://github.com/tessel/t2-firmware)
* [CLI support for JavaScript/Node](https://github.com/tessel/t2-cli/blob/master/lib/tessel/deployment/javascript.js)

### Binary Modules

There is support for precompiled binary modules on Tessel 2. The best way to find out whether the module you want is available is to try deploying it. The module has been precompiled, it will just work!

For modules that have not been precompiled (you can see the list at [packages.tessel.io](https://packages.tessel.io)), you will see an error message explaining why it cannot be loaded:

```
Pre-compiled module is missing: {name of module}.
This might be caused by any of the following:
 1. The binary is platform specific and cannot be compiled for OpenWRT.
 2. A pre-compiled binary has not yet been generated for this module.
 3. The binary didn't compile correctly for the platform that you're developing on.
    It's possible that the binary is Linux-only or even OpenWRT specific,
    try npm installing with "--force" and rerun your deployment command.
Please file an issue at https://github.com/tessel/t2-cli/issues/new
```

Submit an issue and we will look into precompiling it. Our precompilation server lives in the [`t2-compiler` repo](http://github.com/tessel/t2-compiler).

## Rust (work in progress)
Try Rust on Tessel 2 with the following instructions:

Install Tessel 2's command line tool (the same CLI as is used for JavaScript/Node.js) with

```
npm install -g t2-cli
```

You can then create an deploy a blinking lights example using the CLI. Make a folder to work in:

```
mkdir quickstart
cd quickstart
```

In your new folder, automatically initialize all the files you need to run your first Rust code on Tessel 2 with:

```
t2 init --lang=rust
```

You will see a file called `Cargo.toml`, and a folder called `src` containing `main.rs`. `main.rs` is a script to blink some of Tessel's built-in LEDs.

Run the code with

```
t2 run blinky --rustcc
```

The `--rustcc` compiles the Rust script for your Tessel on a remote server.

You use the name `blinky` when you are running your code because that is the name specified in `Cargo.toml`.

The lights on your Tessel 2 should be blinking!

Want to go further? Check out the [tessel-rust repo](https://github.com/tessel/rust-tessel) to see how to do local compilation, use modules, and more.

## Other Languages
Interested in adding support for a new language to Tessel 2's CLI? Here is a detailed blog post on the six components you will need: [Interfacing with the Language Plugin API for Tessel 2](https://tessel.io/blog/148706216397/interfacing-with-the-language-plugin-api-for)

(It is worth noting that [support of Python for Tessel 2](https://github.com/tcr/tessel-python) was originally planned, but is no longer in active development. [Read the blog post](https://tessel.io/blog/146714850172/ramping-up-rust-backing-away-from-python-johnny) explaining this decision.)
