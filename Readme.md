# Cargo bundle

Wrap Rust executables in OS-specific app bundles

## About

`cargo-bundle` is a tool used to generate installers or app bundles for GUI
executables built with `cargo`.  It can create `.app` bundles for Mac OS X and
iOS, `.deb` packages for Linux, and `.msi` installers for Windows (note however
that iOS and Windows support is still experimental).  Support for creating
`.rpm` packages (for Linux) and `.apk` packages (for Android) is still pending.

To install `cargo bundle`, run `cargo install cargo-bundle`. This will add the most recent version of `cargo-bundle`
published to [crates.io](https://crates.io/crates/cargo-bundle) as a subcommand to your default `cargo` installation.

To start using `cargo bundle`, add a `[package.metadata.bundle]` section to your project's `Cargo.toml` file.  This
section describes various attributes of the generated bundle, such as its name, icon, description, copyright, as well
as any packaging scripts you need to generate extra data.  The full manifest format is described below.

To build a bundle for the OS you're on, simply run `cargo bundle` in your
project's directory (where the `Cargo.toml` is placed).  If you would like to
bundle a release build, you must add the `--release` flag to your call.  To
cross-compile and bundle an application for another OS, add an appropriate
`--target` flag, just as you would for `cargo build`.

## Flags

 TODO(burtonageo): Write this

## Bundle manifest format

 There are several fields in the `[package.metadata.bundle]` section.

 * `name`: The name of the built application. If this is not present, then it will use the `name` value from
           your `Cargo.toml` file.
 * `identifier`: [REQUIRED] A string that uniquely identifies your application,
   in reverse-DNS form (for example, `"com.example.appname"` or
   `"io.github.username.project"`).  For OS X and iOS, this is used as the
   bundle's `CFBundleIdentifier` value; for Windows, this is hashed to create
   an application GUID.
 * `icon`: [OPTIONAL] The icons used for your application.  This should be an array of file paths or globs (with images
           in various sizes/formats); `cargo-bundle` will automatically convert between image formats as necessary for
           different platforms.  Supported formats include ICNS, ICO, PNG, and anything else that can be decoded by the
           [`image`](https://crates.io/crates/image) crate.  Icons intended for high-resolution (e.g. Retina) displays
           should have a filename with `@2x` just before the extension (see example below).
 * `version`: [OPTIONAL] The version of the application. If this is not present, then it will use the `version`
              value from your `Cargo.toml` file.
 * `resources`: [OPTIONAL] List of files or directories which will be copied to the resources section of the
                bundle. Globs are supported.
 * `script`: [OPTIONAL] This is a reserved field; at the moment it is not used for anything, but may be used to
             run scripts while packaging the bundle (e.g. download files, compress and encrypt, etc.).
 * `copyright`: [OPTIONAL] This contains a copyright string associated with your application.
 * `short_description`: [OPTIONAL] A short, one-line description of the application. If this is not present, then it
                        will use the `description` value from your `Cargo.toml` file.
 * `long_description`: [OPTIONAL] A longer, multi-line description of the application.

### Example `Cargo.toml`:

```toml
[package]
name = "example"
# ...other fields...

[package.metadata.bundle]
name = "ExampleApplication"
identifier = "com.doe.exampleapplication"
icon = ["32x32.png", "128x128.png", "128x128@2x.png"]
version = "1.0.0"
resources = ["assets", "images/**/*.png", "secrets/public_key.txt"]
copyright = "Copyright (c) Jane Doe 2016. All rights reserved."
short_description = "An example application."
long_description = """
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
eiusmod tempor incididunt ut labore et dolore magna aliqua.  Ut
enim ad minim veniam, quis nostrud exercitation ullamco laboris
nisi ut aliquip ex ea commodo consequat.
"""
```

## Contributing

`cargo-bundle` has ambitions to be inclusive project and welcome contributions from anyone.  Please abide by the Rust
code of conduct.

## Status

Very early alpha. Expect the format of the `[package.metadata.bundle]` section to change, and there is no guarantee of
stability.

## License

This program is licensed either under the terms of the
[Apache Software License](http://www.apache.org/licenses/LICENSE-2.0), or the
[MIT License](https://opensource.org/licenses/MIT).
