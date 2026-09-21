
neovim 설치

```bash
# 만약 Neovim이 이미 설치되어 있다면 제거한다.
sudo apt-get remove neovim -y
```

\# 최신버전 (unstable) 설치
sudo add-apt-repository ppa:neovim-ppa/unstable
sudo apt-get update
sudo apt-get install neovim

```

https://github.com/neovim/neovim/releases/tag/stable
mv /mnt/c/Users/Park\\ Jihoon/Downloads/nvim-linux64.deb ~
sudo apt install ./nvim-linux64.deb
```

nodeJs 설치

```bash
sudo curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Yarn 설치

```bash
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

vim plugin 설치

```bash
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

conda install -c conda-forge pynvim

```

neovim / coc.nvim 설치

```bash
mkdir -p ~/.config/nvim
nvim ~/.config/nvim/init.vim
```

Parkjihoon init.vim
```bash
mv /mnt/d/Downloads/Neovim-setting-win/init.vim .
```




init.vim config 파일

[The config website was moved to vim.fisadev.com](http://nvim.fisadev.com/)

다운받은 파일 마지막에 내용들 추가

```bash

"----------------------------------------------------------------------------------
"-- plugins
"----------------------------------------------------------------------------------
call plug#begin('~/.config/nvim/plugged')

" Use release branch
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" Or latest tag
Plug 'neoclide/coc.nvim', {'tag': '*', 'branch': 'release'}

" Or build from source code by use yarn: <https://yarnpkg.com>
Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}


call plug#end()

```


플러그인 설치

```bash
nvim +PlugInstall

nvim +CocConfig
```

coc 환경 파일 생성

```bash

{
  "codeLens.enable": true,
  "diagnostic.errorSign": "✖",
  "diagnostic.hintSign": "➤",
  "diagnostic.infoSign": "ℹ",
  "diagnostic.warningSign": "⚠",
  "eslint.autoFixOnSave": true,
  "languageserver": {
    "ccls": {
      "command": "ccls",
      "filetypes": [
        "c",
        "cpp",
        "objc",
        "objcpp"
      ],
      "initializationOptions": {
        "cache": {
          "directory": "/tmp/ccls"
        }
      },
      "rootPatterns": [
        ".ccls",
        ".git/",
        ".hg/",
        ".vim/",
        "compile_commands.json"
      ]
    },
   "python": {
      "args": [
        "--log-file",
        "-mpyls",
        "-vv",
        "/tmp/lsp_python.log"
      ],
      "command": "python",
      "filetypes": [
        "python"
      ],
      "settings": {
        "pyls": {
          "commandPath": "",
          "configurationSources": [
            "pycodestyle"
          ],
          "enable": true,
          "plugins": {
            "jedi_completion": {
              "enabled": true
            },
            "jedi_hover": {
              "enabled": true
            },
            "jedi_references": {
              "enabled": true
            },
            "jedi_signature_help": {
              "enabled": true
            },
            "jedi_symbols": {
              "all_scopes": true,
              "enabled": true
            },
            "mccabe": {
              "enabled": true,
              "threshold": 15
            },
            "preload": {
              "enabled": true
            },
            "pycodestyle": {
              "enabled": true
            },
            "pydocstyle": {
              "enabled": false,
              "match": "(?!test_).*\\\\.py",
              "matchDir": "[^\\\\.].*"
            },
            "pyflakes": {
              "enabled": true
            },
            "rope_completion": {
              "enabled": true
            },
            "yapf": {
              "enabled": true
            }
          },
          "trace": {
            "server": "verbose"
          }
        }
      },
      "trace.server": "verbose"
    }
  },
  "python.jediEnabled": false,
  "python.linting.pylintArgs": [
    "--load-plugins",
    "pylint_django"
  ],
  "python.linting.pylintEnabled": true,
  "python.venvFolders": [
    ".direnv",
    ".pyenv",
    "envs",
    "~/.cache/pypoetry/virtualenvs",
    "~/.local/share/virtualenvs"
  ],
  "python.workspaceSymbols.exclusionPatterns": [],
  "signature.enable": true,
  "suggest.echodocSupport": true,
  "suggest.noselect": false,
  "suggest.preferCompleteThanJumpPlaceholder": true,
  "tsserver.implicitProjectConfig.experimentalDecorators": true
}
```

nvim plugin 설치

nvim에 접속한 상태로 아래 커맨드 입력으로 플러그인 설치

```bash
:CocInstall coc-python
:CocInstall coc-pyright
:CocInstall coc-tabnine
:CocInstall coc-json
```
