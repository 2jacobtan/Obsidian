## Command line
`code --diff file1.cs file2.cs`

`code -d file1.cs file2.cs` (shorter)

---

```bash
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff "$LOCAL" "$REMOTE"'
```

^71e330

`git difftool` (try in a git repo)

```bash
git config \--global merge.tool vscode
git config \--global mergetool.vscode.cmd 'code --wait "$MERGED"'
```

`git mergetool` (try in a git repo)

https://www.meziantou.net/comparing-files-using-visual-studio-code.htm
backup [[Comparing files using Visual Studio Code - Gérald Barré]]

https://code.visualstudio.com/docs/editor/command-line

## Themes

themes preview
https://vscodethemes.com/dark

my favourite
https://monokai.pro/
- https://marketplace.visualstudio.com/items?itemName=monokai.theme-monokai-pro-vscode

## Extensions
Vim + Spacemacs https://vspacecode.github.io/
- https://marketplace.visualstudio.com/items?itemName=VSpaceCode.vspacecode

Rainbow Brackets
- https://marketplace.visualstudio.com/items?itemName=2gua.rainbow-brackets

Rainbow Highlighter
- https://marketplace.visualstudio.com/items?itemName=cobaltblu27.rainbow-highlighter

Indent Rainbow
- https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow

Mark lines and jump to them
- https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks

Draw.io Integration
- https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio

Font Switcher
- https://marketplace.visualstudio.com/items?itemName=evan-buss.font-switcher

GitLens
- https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens

Remote Repositories (GitHub)
- https://marketplace.visualstudio.com/items?itemName=github.remotehub

GitHub Pull Requests and Issues
- https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github

Unicode Latex
- https://marketplace.visualstudio.com/items?itemName=oijaz.unicode-latex
## Misc

https://editorconfig.org/
