# Himanshu Verma — Resume

My one-page resume, maintained in LaTeX and compiled with XeLaTeX.

[View the resume PDF](Himanshu_Resume.pdf) · [Website](https://ghostdevhv.me/) · [LinkedIn](https://www.linkedin.com/in/himanshu-verma-1b930990/) · [GitHub](https://github.com/ghostdevhv)

## Project files

| File | Purpose |
| --- | --- |
| `Himanshu_Resume.tex` | Editable resume source |
| `Himanshu_Resume.pdf` | Compiled resume, tracked alongside the source |
| `Fontin*.otf` | Four local font files used by the document |
| `.gitignore` | Excludes build logs, auxiliary files, and local tooling files |

## Get started

```sh
git clone https://github.com/ghostdevhv/resume-latex.git
cd resume-latex
```

Install XeLaTeX and the packages below once, then follow [Compile](#compile). Always build from the project directory so the bundled fonts resolve correctly.

## Install on macOS

Install Homebrew first if `brew` is unavailable, following the [Homebrew instructions](https://brew.sh).

Install BasicTeX (a smaller TeX Live distribution that includes XeLaTeX):

```sh
brew install --cask basictex
```

The installer requires a macOS administrator password. Run this command in an interactive Terminal, or complete the BasicTeX graphical installer. The package commands below use a user-local TeX directory and do not require `sudo`.

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

For system-wide package management instead, keep the TeX Live kernel and package versions compatible. Avoid mixing newly updated core packages with an older LaTeX kernel; the user-local setup above is the configuration verified for this project.

Alternatively, `brew install --cask mactex-no-gui` installs a much larger distribution with a broad package collection, generally avoiding individual package installation.

## Compile

From the project directory (no package installation is needed for subsequent builds):

```sh
xelatex -interaction=nonstopmode -halt-on-error -file-line-error Himanshu_Resume.tex
xelatex -interaction=nonstopmode -halt-on-error -file-line-error Himanshu_Resume.tex
```

After editing `Himanshu_Resume.tex`, run both commands again and review the resulting PDF before committing the source and PDF together. The second pass resolves references and PDF link metadata. The output is `Himanshu_Resume.pdf`; auxiliary files are `.aux`, `.log`, and `.out`.

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
- **FontAwesome cannot be found:** copy its OpenType file to your user Fonts directory as shown above. If an editor cannot see the installed font, restart it and try the build in Terminal.
- **Fontin cannot be found:** confirm all four `.otf` files are beside the source and compile from this directory.
- **Build errors:** inspect `Himanshu_Resume.log`; `-halt-on-error` stops at the first error.
- **Editor preview differs from Terminal:** some editors use a separate compiler or download their own packages. Select XeLaTeX and compare against the local build commands above.

## Verified build (4 November 2026)

BasicTeX 2026.0301 / TeX Live 2026 and XeLaTeX are installed. The additional packages were installed in the user tree, and FontAwesome was copied to the user Fonts directory. The latest header and contact updates compiled successfully with two XeLaTeX passes, producing a one-page `Himanshu_Resume.pdf`. The header contains GitHub and LinkedIn on the left, the name and Bengaluru, India in the center, and email and website on the right; the phone number is omitted.

Remaining nonfatal warnings: obsolete `xcolor` option `usenames`, font-shape substitutions, and an overfull horizontal box of approximately 35.25 pt in the Education table (the Education table uses a fixed width).
