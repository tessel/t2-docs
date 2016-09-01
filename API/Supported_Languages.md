# Supported languages

## JavaScript/Node
Tessel 2 has full support for JavaScript and Node.JS. The relevant repos can be found here:

* [Firmware support for JavaScript/Node](https://github.com/tessel/t2-firmware)
* [CLI support for JavaScript/Node](https://github.com/tessel/t2-cli/blob/master/lib/tessel/deployment/javascript.js)

### Binary modules
There is some support for modules with binary dependencies on Tessel 2.

The best way to find out whether the module you want is supported is to try it. If it's supported, it will work! If it is not supported, you will get an error message like this ([Error message source in CLI](https://github.com/tessel/t2-cli/blob/master/lib/tessel/deployment/javascript.js)):

```
Pre-compiled module is missing: ${name}.
    This might be caused by any of the following:
    1. The binary is platform specific and cannot be compiled for OpenWRT.
    2. A pre-compiled binary has not yet been generated for this module.
    3. The binary didn't compile correctly for the platform that you're developing on.
        It's possible that the binary is Linux-only or even OpenWRT specific,
        try npm installing with "--force" and rerun your deployment command.
    Please file an issue at https://github.com/tessel/t2-cli/issues/new
```

## Rust (work in progress)
The Tessel team is working toward first-class support for Rust. If you're interested in the state of that project, check out the [tessel-rust repo](https://github.com/tessel/rust-tessel).

## How to add support for more languages
Interested in adding support for a new language to Tessel 2's CLI? Here is a detailed blog post on the six components you will need: [Interfacing with the Language Plugin API for Tessel 2](https://tessel.io/blog/148706216397/interfacing-with-the-language-plugin-api-for)

It is worth noting that [support of Python for Tessel 2](https://github.com/tcr/tessel-python) was originally planned, but is no longer in active development. [Read the blog post](https://tessel.io/blog/146714850172/ramping-up-rust-backing-away-from-python-johnny) explaining this decision.
