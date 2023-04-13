

# 如何在vim下配置一个C/C++开发环境

## 补全方案选择

- `clangd` + `coc.nvim`
	- clangd:  llvm-project 下的 一个 LSP
	- coc.nvim : 一款 vim插件
- 为什么使用 `coc` 而不是 `ycm`？
	- `YouCocompleteMe` 可以说是最难安装的一款vim插件，每次安装的体验都可以说是折磨。
	- ycm 半异步(可能)的机制在进行大文本文件分析时变得很卡顿, 亲身体验
	- ycm 是基于 python 的， 而python是单线程的, coc.nvim 是用  type script写的
	- ycm 配置繁杂，而coc有统一的配置文件，且本身也是一个插件管理器



## 配置

### 1. 安装 vim-plug

>https://github.com/junegunn/vim-plug

### 2. 安装 coc.nvim

>https://github.com/neoclide/coc.nvim

**Require** : nodejs>=14.14
			vim >= 8.1.1719
			clangd

更新 node : https://blog.csdn.net/qq_22713201/article/details/122486841

### 3. 安装coc插件

1. clangd / coc-clangd
2. coc-json

### 4. 配置文件
- CocConfig

```json
{
        "languageserver": {
                "clangd": {
                        "command": "clangd",
                        "rootPatterns": [
                                "compile_flags.txt",
                                "compile_commands.json"
                        ],
                        "filetypes": [
                                "c",
                                "cc",
                                "cpp",
                                "c++",
                                "objc",
                                "objcpp"
                        ],
                        "args": [
                                // "--completion-style=bundled", // bundled 补全打包为一个建议, 相反为 detailed
                                "-j=3",
                                "--background-index", // 后台自动分析文件(compile_commands)
                                "--clang-tidy",
                                "--pretty", // 美化输出的json
                                "--function-arg-placeholders=false", // 启用这项时，补全函数时，将会给参数提供占位符，键入后按 Tab 可以切换到下一占位符，乃至函数末k
                                "--enable-config",
                                "--pch-storage=memory",
                                "--query-driver=/bin/gcc"
                        ]
                },
                "golang": {
                        "command": "gopls",
                        "rootPatterns": [
                                "go.work",
                                "go.mod",
                                ".vim/",
                                ".git/",
                                ".hg/"
                        ],
                        "filetypes": [
                                "go"
                        ],
                        "initializationOptions": {
                                "usePlaceholders": false
                        }
                },
                "cmake": {
                        "command": "cmake-language-server",
                        "filetypes": [
                                "cmake"
                        ],
                        "rootPatterns": [
                                "build/"
                        ],
                        "initializationOptions": {
                                "buildDirectory": "build"
                        }
                },
                "haskell": {
                        "command": "haskell-language-server-wrapper",
                        "args": [
                                "--lsp"
                        ],
                        "rootPatterns": [
                                "*.cabal",
                                "stack.yaml",
                                "cabal.project",
                                "package.yaml",
                                "hie.yaml"
                        ],
                        "filetypes": [
                                "haskell",
                                "lhaskell"
                        ],
                        // Settings are optional, here are some example values
                        "settings": {
                                "haskell": {
                                        "checkParents": "CheckOnSave",
                                        "checkProject": true,
                                        "maxCompletions": 10,
                                        "formattingProvider": "ormolu",
                                        "plugin": {
                                                "stan": {
                                                        "globalOn": true
                                                }
                                        }
                                }
                        }
                }
        },
  "@ignored/coc-solidity.telemetry": true
}
```


- vimrc


```sh
call plug#begin()

"Plug 'ycm-core/YouCompleteMe'
Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}
Plug 'scrooloose/nerdtree'  "目录树
Plug 'tpope/vim-surround'
Plug 'jiangmiao/auto-pairs' "括号配对
Plug 'vim-scripts/a.vim'
Plug 'vim-scripts/DoxygenToolkit.vim'
Plug 'voldikss/vim-floaterm'


Plug 'octol/vim-cpp-enhanced-highlight'

Plug 'itchyny/lightline.vim' "status line
Plug 'frazrepo/vim-rainbow' "彩虹括号

" Theme
Plug 'joshdick/onedark.vim'
Plug 'rakr/vim-one'

" All of your Plugins must be added before the following line
call plug#end()            " required



set termguicolors
"tmux Error color setting
" Enable true color 启用终端24位色
if exists('+termguicolors')
   let &t_8f = "\<Esc>[38;2;%lu;%lu;%lum"
   let &t_8b = "\<Esc>[48;2;%lu;%lu;%lum"
   set termguicolors
endif



set laststatus=2
let g:lightline = {
      \ 'component_function': {
      \   'filename': 'LightlineFilename'
      \ }
      \ }
function! LightlineFilename()
  return expand('%')
endfunction





let g:rainbow_active=1

"tab are spaces
"set expandtab


set encoding=utf-8
" Some servers have issues with backup files, see #649.
set nobackup
set nowritebackup
syntax on
syntax enable
set shiftwidth=4
set tabstop=4
set cindent
set number
set foldmethod=indent
set autoindent
set showcmd
set mouse=a
set hlsearch
set incsearch

set path^=/usr/include

set list
set listchars=tab:→\ ,eol:↓

filetype indent on

" Command Line View
set wildmode=list:full
"set wildmenu

set background=dark " for the dark version
" set background=light " for the light version
colorscheme onedark


" Make <TAB> to accept selected completion item or notify coc.nvim to format
" <C-g>u breaks current undo, please make your own choice.
 inoremap <silent><expr> <TAB>  coc#pum#visible() ? coc#pum#confirm():"\<TAB>"


" Use `[g` and `]g` to navigate diagnostics
" Use `:CocDiagnostics` to get all diagnostics of current buffer in location list.
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)

" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)
nmap <leader> rn <Plug>(coc-rename)


" hover show detail or 'K'
nnoremap <silent> K :call ShowDocumentation()<CR>

function! ShowDocumentation()
  if CocAction('hasProvider', 'hover')
    call CocActionAsync('doHover')
  else
    call feedkeys('K', 'in')
  endif
endfunction


" Neartree
map <F3> :NERDTreeMirror<CR>
map <F3> :NERDTreeToggle<CR>
noremap <C-s> :call SaveAndFormat()<CR>
noremap <C-h> <Plug>(coc-fix-current)

noremap <C-t> :call FloatTerm()<CR>
tnoremap   <silent>   <C-t>   <C-\><C-n>:FloatermToggle<CR>


def FloatTerm()
        :w
        :FloatermToggle
enddef

" when open floatterm run in vim (useless)
"autocmd User FloatermOpen  tmux

function SaveAndFormat()
        call CocAction('format')
        :w
endfunction

"nnoremap   <silent>   <F7>    :FloatermNew<CR>
"tnoremap   <silent>   <F7>    <C-\><C-n>:FloatermNew<CR>
"nnoremap   <silent>   <F8>    :FloatermPrev<CR>
"tnoremap   <silent>   <F8>    <C-\><C-n>:FloatermPrev<CR>
"nnoremap   <silent>   <F9>    :FloatermNext<CR>
"tnoremap   <silent>   <F9>    <C-\><C-n>:FloatermNext<CR>
"nnoremap   <silent>   <F12>   :FloatermToggle<CR>
"tnoremap   <silent>   <F12>   <C-\><C-n>:FloatermToggle<CR>


" Highlight the symbol and its references when holding the cursor.
" autocmd CursorHold * silent call CocActionAsync('highlight')

" Mappings for CoCList
" Show all diagnostics.
nnoremap <silent><nowait> <space>a  :<C-u>CocList diagnostics<cr>
" Manage extensions.
nnoremap <silent><nowait> <space>e  :<C-u>CocList extensions<cr>
" Show commands.
nnoremap <silent><nowait> <space>c  :<C-u>CocList commands<cr>
" Find symbol of current document.
nnoremap <silent><nowait> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols.
nnoremap <silent><nowait> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent><nowait> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent><nowait> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list.
nnoremap <silent><nowait> <space>p  :<C-u>CocListResume<CR>




" Status Line Color
"hi User1 cterm=none ctermfg=27 ctermbg=0
"hi User2 cterm=none ctermfg=208 ctermbg=0
"hi User3 cterm=none ctermfg=169 ctermbg=0
"hi User4 cterm=none ctermfg=100 ctermbg=0
"hi User5 cterm=none ctermfg=green ctermbg=0

if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif


" cpp highlight
let g:cpp_class_scope_highlight=1
let g:cpp_variable_highlight=1
let g:cpp_class_decl_highlight=1
let g:cpp_posix_standard = 1
let g:cpp_experimental_simple_template_highlight = 1
let g:cpp_concepts_highlight = 1



" float term config
let g:floaterm_height=0.8
let g:floaterm_with=0.9
"let g:floaterm_wintype=float
let g:floaterm_autoclose=2
```



- bashrc


```bash
#
# ~/.bashrc

# If not running interactively, don't do anything
source /etc/bash.bashrc
[[ $- != *i* ]] && return

alias ls='ls --color=auto'


# ls background color
eval "$(dircolors -p | \
        sed 's/ 4[0-9];/ 01;/; s/;4[0-9];/;01;/g; s/;4[0-9] /;01 /' | \
        dircolors /dev/stdin)"

PROMPT_COMMAND=build_prompt

is_git_repository() {
        git branch > /dev/null 2>&1
}

# Determine the branch/state information for this git repository.
set_git_branch() {
        local state remote branch branch_pattern # keep local

        if ! is_git_repository ; then
                BRANCH=""
                return 1
        fi

# Capture the output of the "git status" command.
                git_status="$(git status 2> /dev/null)"

                # Set color based on clean/staged/dirty.
                if [[ ${git_status} =~ "working tree clean" ]]; then
                        state="${GREEN}"
                elif [[ ${git_status} =~ "Changes to be committed" ]]; then
                        state="${YELLOW}"
                else
                        state="${RED}"
                fi

                # Set arrow icon based on status against remote.
                remote_pattern="# Your branch is (ahead|behind)+ "
                if [[ ${git_status} =~ ${remote_pattern} ]]; then
                        if [[ ${BASH_REMATCH[1]} == "ahead" ]]; then
                                remote="↑"
                        else
                                remote="↓"
                        fi
                else
                        remote=""
                fi

                diverge_pattern="# Your branch and (.*) have diverged"
                if [[ ${git_status} =~ ${diverge_pattern} ]]; then
                        remote="↕"
                fi

          # Get the name of the branch.
          branch_pattern="^(# )?On branch ([^${IFS}]*)"
          if [[ ${git_status} =~ ${branch_pattern} ]]; then
                  branch=${BASH_REMATCH[2]}
          fi

          # Set the final branch string.
          BRANCH=${state}"(${branch})"${remote}${NC}
}


build_prompt() {
  EXIT=$?               # save exit code of last command

   green='\[\e[0;32m\]'
   cyan='\[\e[1;36m\]'
   diy='\[\e[0;25m\]'
   purple='\[\e[0;36m\]'


   YELLOW='\[\e[1;33m\]'
   GREEN='\[\e[0;32m\]'
   BLUE='\[\e[1;34m\]'
   RED='\[\e[0;31m\]'
   LIGHT_RED='\[\e[1;31m\]'
   LIGHT_GREEN='\[\e[1;32m\]'
   WHITE='\[\e[1;37m\]'
   LIGHT_GRAY='\[\e[0;37m\]'


  PS1='${debian_chroot:+($debian_chroot)}'  # begin prompt

  if [ $EXIT != 0 ]; then  # add arrow color dependent on exit code
    PS1+="$red"
  else
    PS1+="$green"
  fi

  local BRANCH
  set_git_branch

  if is_git_repository ; then
          BRANCH=$WHITE:${BRANCH}
  fi

  #PS1+="→$reset  $cyan\W$reset \\$ " # construct rest of prompt
  #PS1+="→$reset  $cyan\W$reset:$reset$WHITE${BRANCH}$diy\$ " # construct rest of prompt
  PS1+="→  $cyan\W${BRANCH}$WHITE \$ " # construct rest of prompt
}











#export DISPLAY=172.31.208.1:0
export PYTHONSTARTUP=~/.pythonstartup
export PATH="$PATH:$(go env GOPATH)/bin"
export PATH="$PATH:$(go env GOROOT)/bin"
export PATH=$PATH:/home/gloria/.local/bin
export PATH=$PATH:/usr/lib/wsl/lib
export PATH=$PATH:~/.ghcup/bin
export PATH=$PATH:"/mnt/d/Program Files/Microsoft/Microsoft VS Code Insiders/bin/"
export PATH=$PATH:"/mnt/d/Program Files/Microsoft/Microsoft VS Code/bin/"
export PATH=$PATH:~/.yarn/bin/

export PYENV_ROOR="~/.pyenv"
export PATH=$PYENV_ROOT/shims:$PATH
eval "$(pyenv init -)"
#eval "$(pyenv virtualenv-init -)"
eval "$(thefuck --alias)"



source /mbin/win_proxy.sh set
alias w_proxy="source /mbin/win_proxy.sh"
source /mbin/change_comiler_alias.sh clang
alias set_c_toolchain="source /mbin/change_comiler_alias.sh"



# Cargo cannot add
export CARGO_NET_GIT_FETCH_WITH_CLI=true

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk

alias xmk=xmake
export NODE_PATH=/usr/lib/node_modules

alias gr="go run"
alias py="python"
alias tat="tmux attach -t"
alias sshRoot="ssh root@godot.link"
alias dk=docker
alias pm=pacman
alias nhh='npx hardhat'

alias v=vim
```


