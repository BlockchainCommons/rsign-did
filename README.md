# rsign2

A Rust implementation of [Minisign](https://jedisct1.github.io/minisign/).

All signatures produced by `rsign` can be verified with `minisign` including trusted comments.

And `minisign` is able to sign files with keys generated by `rsign2`.

In Rust, signatures can also be verified with the [minisign-verify](https://docs.rs/minisign-verify) crate.

`rsign2` is a maintained fork of [`rsign`](https://docs.rs/crate/rsign/), originally written by Daniel Rangel.

Main differences with rsign:

- `rsign2` is written in pure Rust.
- `rsign2` has way less dependencies.
- `rsign2` includes bug fixes and improvements.
- `rsign2` tries to be usable as a library, not just as a command-line tool.
- `rsign2` supports WebAssembly.

## API documentation

`rsign2` is only a command-line interface. It relies on the Minisign crate, that can be embedded in any application:

[API documentation on docs.rs](https://docs.rs/minisign)

## Usage

```sh
rsign generate
```

Generates a new key pair. The public key is printed in the screen and stored in `rsign.pub` by default. The secret key will be written at `~/.rsign/rsign.key`. You can change the default paths with `-p` and `-s` respectively.

```sh
rsign sign myfile.txt
```

Sign `myfile.txt` with your secret key. You can add a signed trusted comment with:

```sh
rsign sign myfile.txt -t "my trusted comment"
```

If you are signing files larger than 1Gb you must use `-H` to first hash the file and sign the hash after that:

```sh
rsign sign mylargefile.bin -H
```

And to verify the signature with a given public key you can use:

```sh
rsign verify myfile.txt -p rsign.pub
```

Or if you have saved the signature file with a custom name other than `myfile.txt.minisig` and want to use a public key string you can use:

```sh
rsign verify myfile.txt -P [PUBLIC KEY STRING] -x mysignature.file
```

You can find more information using the help subcommand as in:

```text
rsign help [SUBCOMMAND]

USAGE:
    rsign [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    generate    Generate public and private keys
    help        Prints this message or the help of the given subcommand(s)
    sign        Sign a file with a given private key
    verify      Verify a signed file with a given public key
```
