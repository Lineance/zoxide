<!-- markdownlint-configure-file {
  "MD013": {
    "code_blocks": false,
    "tables": false
  },
  "MD033": false,
  "MD041": false
} -->

<div align="center">

<h1>
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./contrib/logo-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="./contrib/logo-light.svg">
    <img alt="zoxide-romanized logo" src="./contrib/logo-light.svg" height="128" width="128">
  </picture>
  <br />
  zoxide-romanized
</h1>

[![crates.io][crates.io-badge]][crates.io]
[![Downloads][downloads-badge]][releases]
[![Built with Nix][builtwithnix-badge]][builtwithnix]

zoxide-romanized is the multi-language romanization edition of [zoxide][upstream].
It provides the shared romanizer framework. Chinese pinyin is the first and
currently only implemented language, with full-pinyin and initials matching.
The design allows additional language romanizers to be added independently.

- `z shichang` matches `市场` by full pinyin.
- `z sc` matches `市场` by pinyin initials.
- `z sy2` matches `实验2`, preserving the numeric suffix.
- Polyphonic readings are considered for both full pinyin and initials.

The installed executable and shell command remain `zoxide` / `z`.

[Getting started](#getting-started) •
[Installation](#installation) •
[Configuration](#configuration) •
[Integrations](#third-party-integrations)

</div>

## Getting started

![Tutorial][tutorial]

```sh
z foo              # cd into highest ranked directory matching foo
z foo bar          # cd into highest ranked directory matching foo and bar
z foo /            # cd into a subdirectory starting with foo

z ~/foo            # z also works like a regular cd command
z foo/             # cd into relative path
z ..               # cd one level up
z -                # cd into previous directory

zi foo             # cd with interactive selection (using fzf)

z foo<SPACE><TAB>  # show interactive completions (bash 4.4+/fish/zsh only)
```

Read more about the matching algorithm [here][algorithm-matching].

## Installation

The primary multi-language crate currently enables pinyin by default:

```sh
cargo install zoxide-romanized --locked
```

`zoxide-pinyin` is the Chinese-only edition. It disables the primary crate's
default features and enables only pinyin, so future language features in the
primary crate will not be included:

```sh
cargo install zoxide-pinyin --locked
```

Install only one crate. Both install the executable named `zoxide`. If Cargo
reports that an existing `zoxide` binary would be overwritten, use
`cargo install zoxide-romanized --locked --force` to replace it. Prebuilt builds
from this fork, when available, are listed on the [Lineance releases][releases]
page. On Unix, the installer fetches the matching fork build:

```sh
curl -sSfL https://raw.githubusercontent.com/Lineance/zoxide/main/install.sh | sh
```

The shell setup remains the usual `zoxide init` command. Your existing
zoxide shell configuration and database are reused automatically because this
fork uses the same executable name and data directory. Legacy `db.zo` databases
are migrated to `db.txt` on first read; no manual conversion is needed.

1. **Setup zoxide on your shell**

   To start using zoxide, add it to your shell.

   <details>
   <summary>Bash</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually `~/.bashrc`):
   >
   > ```sh
   > eval "$(zoxide init bash)"
   > ```

   </details>

   <details>
   <summary>Elvish</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually `~/.elvish/rc.elv`):
   >
   > ```sh
   > eval (zoxide init elvish | slurp)
   > ```
   >
   > **Note:**
   > zoxide only supports elvish v0.18.0 and above.

   </details>

   <details>
   <summary>Fish</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually
   > `~/.config/fish/config.fish`):
   >
   > ```sh
   > zoxide init fish | source
   > ```

   </details>

   <details>
   <summary>Nushell</summary>

   > Add this to the <ins>**end**</ins> of your env file (find it by running `$nu.env-path`
   > in Nushell):
   >
   > ```sh
   > zoxide init nushell | save -f ~/.zoxide.nu
   > ```
   >
   > Now, add this to the <ins>**end**</ins> of your config file (find it by running
   > `$nu.config-path` in Nushell):
   >
   > ```sh
   > source ~/.zoxide.nu
   > ```
   >
   > **Note:**
   > zoxide only supports Nushell v0.106.0+.

   </details>

   <details>
   <summary>PowerShell</summary>

   > Add this to the <ins>**end**</ins> of your config file (find it by running
   > `echo $profile` in PowerShell):
   >
   > ```powershell
   > Invoke-Expression (& { (zoxide init powershell | Out-String) })
   > ```

   </details>

   <details>
   <summary>Tcsh</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually `~/.tcshrc`):
   >
   > ```sh
   > zoxide init tcsh > ~/.zoxide.tcsh
   > source ~/.zoxide.tcsh
   > ```

   </details>

   <details>
   <summary>Xonsh</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually `~/.xonshrc`):
   >
   > ```python
   > execx($(zoxide init xonsh), 'exec', __xonsh__.ctx, filename='zoxide')
   > ```

   </details>

   <details>
   <summary>Zsh</summary>

   > Add this to the <ins>**end**</ins> of your config file (usually `~/.zshrc`):
   >
   > ```sh
   > eval "$(zoxide init zsh)"
   > ```
   >
   > For completions to work, the above line must be added _after_ `compinit` is
   > called. You may have to rebuild your completions cache by running
   > `rm ~/.zcompdump*; compinit`.

   </details>

   <details>
   <summary>Any POSIX shell</summary>

   > Add this to the <ins>**end**</ins> of your config file:
   >
   > ```sh
   > eval "$(zoxide init posix --hook prompt)"
   > ```

   </details>

   > **Note:**
   > [Warp] provides its own completions, so `Space+Tab` completions are not
   > supported there.

2. **Install fzf** <sup>(optional)</sup>

   [fzf] is a command-line fuzzy finder, used by zoxide for completions /
   interactive selection. It can be installed from [here][fzf-installation].

   > **Note:**
   > The minimum supported fzf version is v0.51.0.

3. **Import your data** <sup>(optional)</sup>

   If you currently use any of these plugins, you may want to import your data
   into zoxide. The data file is auto-detected using each plugin's standard
   conventions.

   ```sh
   zoxide import <plugin>
   ```

   | Plugin   | Command                  |
   | -------- | ------------------------ |
   | atuin    | `zoxide import atuin`    |
   | autojump | `zoxide import autojump` |
   | fasd     | `zoxide import fasd`     |
   | z        | `zoxide import z`        |
   | z.lua    | `zoxide import z.lua`    |
   | zsh-z    | `zoxide import zsh-z`    |

## Configuration

### Flags

When calling `zoxide init`, the following flags are available:

- `--cmd`
  - Changes the prefix of the `z` and `zi` commands.
  - `--cmd j` would change the commands to (`j`, `ji`).
  - `--cmd cd` would replace the `cd` command.
- `--hook <HOOK>`
  - Changes how often zoxide increments a directory's score:

    | Hook            | Description                       |
    | --------------- | --------------------------------- |
    | `none`          | Never                             |
    | `prompt`        | At every shell prompt             |
    | `pwd` (default) | Whenever the directory is changed |

- `--no-cmd`
  - Prevents zoxide from defining the `z` and `zi` commands.
  - These functions will still be available in your shell as `__zoxide_z` and
    `__zoxide_zi`, should you choose to redefine them.

### Environment variables

Environment variables[^2] can be used for configuration. They must be set before
`zoxide init` is called.

- `_ZO_DATA_DIR`
  - Specifies the directory in which the database is stored.
  - The default value varies across OSes:

    | OS          | Path                                                   | Example                                           |
    | ----------- | ------------------------------------------------------ | ------------------------------------------------- |
    | Linux / BSD | `$XDG_DATA_HOME/zoxide` or `$HOME/.local/share/zoxide` | `/home/alice/.local/share/zoxide`                 |
    | macOS       | `$HOME/Library/Application Support/zoxide`             | `/Users/Alice/Library/Application Support/zoxide` |
    | Windows     | `%LOCALAPPDATA%\zoxide`                                | `C:\Users\Alice\AppData\Local\zoxide`             |

- `_ZO_ECHO`
  - When set to 1, `z` will print the matched directory before navigating to
    it.
- `_ZO_EXCLUDE_DIRS`
  - Excludes the specified directories from the database.
  - This is provided as a list of [globs][glob], separated by OS-specific
    characters:

    | OS                  | Separator | Example                 |
    | ------------------- | --------- | ----------------------- |
    | Linux / macOS / BSD | `:`       | `$HOME:$HOME/private/*` |
    | Windows             | `;`       | `$HOME;$HOME/private/*` |

  - By default, this is set to `"$HOME"`.
- `_ZO_FZF_OPTS`
  - Custom options to pass to [fzf] during interactive selection. See
    [`man fzf`][fzf-man] for the list of options.
- `_ZO_MAXAGE`
  - Configures the [aging algorithm][algorithm-aging], which limits the maximum
    number of entries in the database.
  - By default, this is set to 10000.
- `_ZO_RESOLVE_SYMLINKS`
  - When set to 1, `z` will resolve symlinks before adding directories to the
    database.

### Romanization features

`zoxide-romanized` is the multi-language edition and provides the shared
romanizer framework. Pinyin is currently the only implemented language and
is enabled by default.
ASCII letters and digits can be mixed in a romanized query (for example, `sy2`
matches `实验2`). Future language romanizers can be exposed as separate Cargo
features.

`zoxide-pinyin` is a focused Chinese edition. It depends on
`zoxide-romanized` with default features disabled and enables only the `pinyin`
feature. This keeps the pinyin package independent of future romanizer features.
Both packages install the same `zoxide` executable; install only one.

## Third-party integrations

| Application           | Description                                  | Plugin                                  |
| --------------------- | -------------------------------------------- | --------------------------------------- |
| [aerc]                | Email client                                 | Natively supported                      |
| [alfred]              | macOS launcher                               | [alfred-zoxide]                         |
| [clink]               | Improved cmd.exe for Windows                 | [clink-zoxide]                          |
| [emacs]               | Text editor                                  | [zoxide.el]                             |
| [felix]               | File manager                                 | Natively supported                      |
| [joshuto]             | File manager                                 | Natively supported                      |
| [lf]                  | File manager                                 | See the [wiki][lf-wiki]                 |
| [nnn]                 | File manager                                 | [nnn-autojump]                          |
| [ranger]              | File manager                                 | [ranger-zoxide]                         |
| [raycast]             | macOS launcher                               | [raycast-zoxide]                        |
| [rfm]                 | File manager                                 | Natively supported                      |
| [sesh]                | `tmux` session manager                       | Natively supported                      |
| [superfile]           | File manager                                 | [Natively supported][superfile-plugins] |
| [telescope.nvim]      | Fuzzy finder for Neovim                      | [telescope-zoxide]                      |
| [tmux-session-wizard] | `tmux` session manager                       | Natively supported                      |
| [tmux-sessionx]       | `tmux` session manager                       | Natively supported                      |
| [vim] / [neovim]      | Text editor                                  | [zoxide.vim]                            |
| [xplr]                | File manager                                 | [zoxide.xplr]                           |
| [xxh]                 | Transports shell configuration over SSH      | [xxh-plugin-prerun-zoxide]              |
| [yazi]                | File manager                                 | Natively supported                      |
| [zabb]                | Finds the shortest possible query for a path | Natively supported                      |
| [zesh]                | `zellij` session manager                     | Natively supported                      |
| [zsh-autocomplete]    | Realtime completions for zsh                 | Natively supported                      |

[^2]:
    If you're not sure how to set an environment variable on your shell, check
    out the [wiki][wiki-env].

[aerc]: https://github.com/rjarry/aerc
[alfred]: https://www.alfredapp.com/
[alfred-zoxide]: https://github.com/yihou/alfred-zoxide
[algorithm-aging]: https://github.com/ajeetdsouza/zoxide/wiki/Algorithm#aging
[algorithm-matching]: https://github.com/ajeetdsouza/zoxide/wiki/Algorithm#matching
[builtwithnix-badge]: https://img.shields.io/badge/builtwith-nix-7d81f7?logo=nixos&logoColor=white&style=flat-square
[builtwithnix]: https://builtwithnix.org/
[clink-zoxide]: https://github.com/shunsambongi/clink-zoxide
[clink]: https://github.com/mridgers/clink
[crates.io-badge]: https://img.shields.io/crates/v/zoxide-romanized?logo=rust&logoColor=white&style=flat-square
[crates.io]: https://crates.io/crates/zoxide-romanized
[downloads-badge]: https://img.shields.io/github/downloads/Lineance/zoxide/total?logo=github&logoColor=white&style=flat-square
[emacs]: https://www.gnu.org/software/emacs/
[felix]: https://github.com/kyoheiu/felix
[fzf-installation]: https://github.com/junegunn/fzf#installation
[fzf-man]: https://manpages.ubuntu.com/manpages/en/man1/fzf.1.html
[fzf]: https://github.com/junegunn/fzf
[glob]: https://man7.org/linux/man-pages/man7/glob.7.html
[issues]: https://github.com/Lineance/zoxide/issues/new
[joshuto]: https://github.com/kamiyaa/joshuto
[lf]: https://github.com/gokcehan/lf
[lf-wiki]: https://github.com/gokcehan/lf/wiki/Integrations#zoxide
[neovim]: https://github.com/neovim/neovim
[nnn-autojump]: https://github.com/jarun/nnn/blob/master/plugins/autojump
[nnn]: https://github.com/jarun/nnn
[ranger-zoxide]: https://github.com/jchook/ranger-zoxide
[ranger]: https://github.com/ranger/ranger
[raycast]: https://www.raycast.com/
[raycast-zoxide]: https://www.raycast.com/mrpunkin/raycast-zoxide
[releases]: https://github.com/Lineance/zoxide/releases
[rfm]: https://github.com/dsxmachina/rfm
[sesh]: https://github.com/joshmedeski/sesh
[superfile]: https://github.com/yorukot/superfile
[superfile-plugins]: https://superfile.dev/list/plugin-list/
[telescope-zoxide]: https://github.com/jvgrootveld/telescope-zoxide
[telescope.nvim]: https://github.com/nvim-telescope/telescope.nvim
[tmux-session-wizard]: https://github.com/27medkamal/tmux-session-wizard
[tmux-sessionx]: https://github.com/omerxx/tmux-sessionx
[tutorial]: contrib/tutorial.gif
[upstream]: https://github.com/ajeetdsouza/zoxide
[vim]: https://github.com/vim/vim
[wiki-env]: https://github.com/ajeetdsouza/zoxide/wiki/HOWTO:-set-environment-variables "HOWTO: set environment variables"
[xplr]: https://github.com/sayanarijit/xplr
[xxh-plugin-prerun-zoxide]: https://github.com/xxh/xxh-plugin-prerun-zoxide
[xxh]: https://github.com/xxh/xxh
[yazi]: https://github.com/sxyazi/yazi
[zabb]: https://github.com/Mellbourn/zabb
[zesh]: https://github.com/roberte777/zesh
[zoxide.el]: https://gitlab.com/Vonfry/zoxide.el
[zoxide.vim]: https://github.com/nanotee/zoxide.vim
[zoxide.xplr]: https://github.com/sayanarijit/zoxide.xplr
[zsh-autocomplete]: https://github.com/marlonrichert/zsh-autocomplete
