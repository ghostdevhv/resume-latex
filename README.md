# Himanshu Verma — Resume

LaTeX source: `Himanshu_Resume.tex`. The project uses XeLaTeX and the four Fontin font files included in this directory. Run builds from this directory so the fonts resolve correctly.

## Install on macOS

Install Homebrew first if `brew` is unavailable, following the [Homebrew instructions](https://brew.sh).

Install BasicTeX (a smaller TeX Live distribution that includes XeLaTeX):

```sh
brew install --cask basictex
```

The installer requires a macOS administrator password. Run this command in an interactive Terminal, or complete the BasicTeX graphical installer. Do not enter your password into chat.

Restart Terminal after installation, or enable TeX commands in the current shell:

```sh
export PATH="/Library/TeX/texbin:$PATH"
xelatex --version
```

Install the extra packages in your user TeX directory (the method verified on this machine):

```sh
/Library/TeX/texbin/tlmgr init-usertree
/Library/TeX/texbin/tlmgr --usermode install fontspec xunicode xltxtra realscripts metalogo parskip xcolor geometry titlesec fontawesome enumitem hyperref url greek-fontenc
mkdir -p "$HOME/Library/Fonts"
cp "$(/Library/TeX/texbin/kpsewhich FontAwesome.otf)" "$HOME/Library/Fonts/FontAwesome.otf"
```

Run `init-usertree` only once. User packages are stored under `~/Library/texmf`. The FontAwesome copy makes the icon font available to macOS font services. BasicTeX already includes `extsizes`, `tools` (including `multicol` and `tabularx`), and the standard graphics/base packages.

Keep BasicTeX's bundled `tools` version: the repository version tested on 27 October 2026 required a newer LaTeX kernel than BasicTeX 2026.0301 provides. If you installed it into the user tree and encounter an undefined `\vcenter@text` command, restore the compatible bundled version with:

```sh
mkdir -p "$HOME/Library/texmf/tlpkg/backups"
/Library/TeX/texbin/tlmgr --usermode remove tools
```

For a system-wide installation instead, run `sudo tlmgr update --self` and `sudo tlmgr update --all` together before installing missing packages, so the kernel and packages stay compatible. These commands require an administrator password in Terminal.

Alternatively, `brew install --cask mactex-no-gui` installs a much larger distribution with a broad package collection, generally avoiding individual package installation.

## Compile

From the project directory (no package installation is needed for subsequent builds):

```sh
xelatex -interaction=nonstopmode -halt-on-error -file-line-error Himanshu_Resume.tex
xelatex -interaction=nonstopmode -halt-on-error -file-line-error Himanshu_Resume.tex
```

The second pass resolves references and PDF link metadata. The output is `Himanshu_Resume.pdf`; auxiliary files are `.aux`, `.log`, and `.out`.

Use XeLaTeX rather than pdfLaTeX: `fontspec` loads the bundled OpenType fonts. No system-wide Fontin installation is required.

## Dependencies used by the source

| Purpose | LaTeX packages / class |
| --- | --- |
| Document class | `extarticle` (`extsizes`) |
| Fonts and Unicode | `fontspec`, `xunicode`, `xltxtra` |
| Icons | `fontawesome` |
| Page and section layout | `geometry`, `titlesec`, `parskip`, `multicol` |
| Tables and lists | `tabularx` (`tools`), `enumitem` |
| Color and graphics | `color`, `graphicx` (standard graphics bundle), `xcolor` |
| Links and symbols | `hyperref`, `url`, `textcomp` |

## Troubleshooting

- **`xelatex: command not found`:** restart Terminal or set the PATH shown above. The binary should be `/Library/TeX/texbin/xelatex`.
- **`File ...sty not found`:** install the corresponding package with `/Library/TeX/texbin/tlmgr --usermode install PACKAGE`. Use `/Library/TeX/texbin/tlmgr --usermode search --global --file '/FILENAME.sty'` to find its package. Keep the bundled `tools` version as explained above.
- **FontAwesome cannot be found:** copy its OpenType file to your user Fonts directory as shown above. In a sandboxed command runner, font services may require running the build with approved access outside the sandbox.
- **Fontin cannot be found:** confirm all four `.otf` files are beside the source and compile from this directory.
- **Build errors:** inspect `Himanshu_Resume.log`; `-halt-on-error` stops at the first error.
- **Editor preview download timeout:** the built-in editor compiler downloads its own dependencies and may operate independently of a local TeX installation. The local XeLaTeX commands above are verified to work; a successful terminal build does not confirm that the editor's separate compiler has finished downloading its dependencies.

## Setup status (27 October 2026)

BasicTeX 2026.0301 / TeX Live 2026 and XeLaTeX are installed. The additional packages were installed in the user tree, and FontAwesome was copied to the user Fonts directory. The resume compiled successfully with two XeLaTeX passes, producing a one-page `Himanshu_Resume.pdf`, without source edits.

Remaining nonfatal warnings: obsolete `xcolor` option `usenames`, font-shape substitutions, and an overfull horizontal box of approximately 35.25 pt in the Education table (source lines 310–314).
