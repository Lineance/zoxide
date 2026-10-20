# zoxide-pinyin

Compatibility package pinned to the matching `zoxide-romanized` release. It
disables the primary package's default features and enables only `pinyin`, so
additional romanizers added to the primary package do not become dependencies
of this package.

Installing this crate provides the `zoxide` executable with pinyin matching
enabled by default:

```sh
cargo install zoxide-pinyin --locked
```

Do not install this package and `zoxide-romanized` side by side; both provide
the `zoxide` executable.
