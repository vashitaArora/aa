## Install Git for Windows

**Manual Steps:**

  - [Git for Windows](http://git-scm.com/download/win) is the standalone
    Git command line client for Windows.
      - Recommended: Choose the "Use Git from the Windows Command
        Prompt" option when prompted.
      - Important: Leave "Checkout Windows-style, commit Unix-style line
        endings" option selected. (This sets `core.autocrlf` to `true`,
        because otherwise you might get compilation failing with `error
        CS1040: Preprocessor directives must appear as the first
        non-whitespace character on a line`)
      - For "Configuring the terminal emulator to use with GitBash", the
        default is fine.
      - For "Configuring extra options", leave the defaults to "Enable
        file system caching" and "Enable Git Credential Manager".
      - Run the following commands to set up your name and email for
        commits:
            git config --global user.name "Your Name"
            git config --global user.email you@microsoft.com
      - Run the following command to ensure that you won't lose data
        through line-ending corrections:
            git config --global core.safecrlf true
      - Recommended: Opt into a safer behavior where "git push" only
        pushes commits in your current branch to the server: ([for more
        info](https://github.com/miroadamy/miroadamy-dot-com/wiki/Difference-between-matching-and-simple---Git-push))
            git config --global push.default simple
      - Recommended: Enable fscache and preloadindex flags to improve
        performance on Windows:
            git config --global core.preloadindex true
            git config --global core.fscache true
      - (Optional, Not Recommended) Configure wordpad or notepad++ as
        your default editor since they handle
            \\n:
            git config --global core.editor "'C:\Program Files (x86)\Windows NT\Accessories\wordpad.exe'"
            git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
      - (Optional) Enable git rerere to help on large rebase operations
            git config --global rerere.enabled true
  - (Optional) [Posh-git](https://github.com/dahlbyk/posh-git) provides
    PowerShell integration for Git
      - Note: For large repos, you will want to set
        '$GitPromptSettings.EnableFileStatus = $false' in your
        powershell profile.
  - (Optional) Configure Git to not warn about CRLF line endings when
    diffing changes. Run the following command in your local Git
    repository, or include the --global option to set globally:
        git config --global core.whitespace cr-at-eol
