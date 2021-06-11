---
created: 2021-06-11T15:52:05 (UTC +08:00)
tags: []
source: https://www.meziantou.net/comparing-files-using-visual-studio-code.htm
author: 
---

# Comparing files using Visual Studio Code - Meziantou's blog

> ## Excerpt
> In this post, I describe how to use Visual Studio Code to compare 2 files and also how to use VS Code as a git diff tool.

---
In the previous post, I showed you how to [use Visual Studio to compare 2 files](https://www.meziantou.net/comparing-files-using-visual-studio.htm "Comparing files using Visual Studio"). I also use Visual Studio Code from time to time. And, as every great IDE, Visual Studio Code also has a great diff tool. As with Visual Studio, you can use it to compare 2 versions of the same file if you use a source control. But you can also compare 2 files from your file system.

There are multiple ways to use the Visual Studio Code diff tool:

-   [Comparing files using the User Interface](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#comparing-files-usin)
-   [Comparing files using the command line](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#comparing-files-usin-7cf2ec)
-   [Using Visual Studio Code as a git difftool](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#using-visual-studio-7cf2ec)

## [#](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#comparing-files-usin)Comparing files using the User Interface

1.  Open the 2 files in Visual Studio Code
    
2.  Right-click on one file and click "Select for compare"
    
    ![](https://www.meziantou.net/assets/selectforcompare.png?v=3be0)
    
3.  Right-click on the other file and click "Compare file file1 with file2"
    
    ![](https://www.meziantou.net/assets/compare-2.png?v=0f86)
    

You should see the result:

[![](https://www.meziantou.net/assets/result-2.png?v=294c)](https://www.meziantou.net/assets/result-2.png?v=294c)

## [#](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#comparing-files-usin-7cf2ec)Comparing files using the command line

-   Using Visual Studio Code
    
    ```
    code --diff file1.cs file2.cs
    ```
    
    Or, using the full path if code is not in the `PATH`:
    
    ```
    "%LOCALAPPDATA%\Programs\Microsoft VS Code\code.exe" --diff file1.cs file2.cs
    ```
    
-   Using Visual Studio Code Insiders
    
    ```
    "%LOCALAPPDATA%\Programs\Microsoft VS Code Insiders\Code - Insiders.exe" --diff file1.cs file2.cs
    ```
    

[![](https://www.meziantou.net/assets/result-2.png?v=294c)](https://www.meziantou.net/assets/result-2.png?v=294c)

## [#](https://www.meziantou.net/comparing-files-using-visual-studio-code.htm#using-visual-studio-7cf2ec)Using Visual Studio Code as a git difftool

`git difftool` is a Git command that allows you to compare and edit files between revisions using common diff tools. You can use any editor that supports diff such as VS Code. Open a command prompt and use the following commands:

-   From CMD (Windows)
    
    ```
    git config --global diff.tool vscode
    git config --global difftool.vscode.cmd "code --wait --diff \"$LOCAL\" \"$REMOTE\""
    ```
    
-   From PowerShell (Windows or Linux)
    
    ```
    git config --global diff.tool vscode
    git config --global difftool.vscode.cmd "code --wait --diff ""`$LOCAL"" ""`$REMOTE"""
    ```
    
-   From Bash (Linux)
    
    ```
    git config --global diff.tool vscode
    git config --global difftool.vscode.cmd 'code --wait --diff "$LOCAL" "$REMOTE"'
    ```
    
-   Manually, open `~/.gitconfig` or `%USERPROFILE%\.gitconfig` in a text editor and add the following content:
    
    ```
    [diff]
        tool = vscode
    [difftool "vscode"]
        cmd = code --wait --diff \"$LOCAL\" \"$REMOTE\"
    ```
    

You can validate the configuration by executing `git config --global --list`.

By the way, you can also use Visual Studio Code as your git editor:

```
git config --global core.editor "code --wait"
```

Do you have a question or a suggestion about this post? [Contact me!](https://www.meziantou.net/contact.htm)
