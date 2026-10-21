# zoxide-pinyin

The Chinese-only edition of `zoxide-romanized`. It disables the primary
crate's default features and enables only `pinyin`, so future romanizer features
will not be pulled into this package.

Install this package for Chinese full-pinyin and initials matching:

```sh
cargo install zoxide-pinyin --locked
```

Do not install this package and `zoxide-romanized` side by side; both provide
the `zoxide` executable.
