# scoop-cronyx

A [Scoop](https://scoop.sh) bucket for the [Cronyx](https://github.com/brendancron/CronyxLang)
toolchain.

```
scoop bucket add cronyx https://github.com/brendancron/scoop-cronyx
scoop install cronyx
```

That installs one complete toolchain: `cx`, the compiler it links rather than
shells out to, and the standard library it resolves `import "std/…"` against.
There is nothing further to install to start writing Cronyx.

```
scoop update cronyx
```

## What the manifest does that most do not

`cx` looks for the standard library at `..\lib\cronyx\stdlib`, relative to its
own binary. Scoop puts a generated *shim* on the `PATH` rather than a link, and
a shim is a different executable that reports itself — so a search starting from
the running binary would begin in the shims directory and find nothing. The
manifest sets `CRONYX_STDLIB` instead, which is the override `cx` checks first.

`cronyxc`, the bare compiler driver, is deliberately left off the `PATH`. `cx`
is the tool; `cronyxc` is for debugging the compiler itself. The Homebrew
formula makes the same choice.

## Keeping up with releases

`.github/workflows/update.yml` points the manifest at the newest release that
carries a Windows archive, once a day and on demand. Scoop's `checkver` and
`autoupdate` fields are declarations rather than behaviour — nothing reads them
unless something runs the update — so this bucket runs it itself rather than
relying on a scheduled job it does not have.
