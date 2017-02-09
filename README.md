# INSTALLATION NeoVim

## 1. install on Mac
```
brew install neovim/neovim/neovim
```

## 2. add plugins into init.vim and then NeoBundleCheckUpdate
```
set nocompatible
" NeoBundle Scripts-----------------------------
set langmenu=en_US.UTF-8
let $LANG = 'en'
" let mapleader = "\<space>"
let mapleader = ","
filetype plugin indent on

if has('vim_starting')
  set runtimepath+=~/.config/nvim/bundle/neobundle.vim/
  set runtimepath+=~/.config/nvim/
endif

let neobundle_readme=expand('~/.config/nvim/bundle/neobundle.vim/README.md')

if !filereadable(neobundle_readme)
  echo "Installing NeoBundle..."
  echo ""
  silent !mkdir -p ~/.config/nvim/bundle
  silent !git clone https://github.com/Shougo/neobundle.vim ~/.config/nvim/bundle/neobundle.vim/
  let g:not_finsh_neobundle = "yes"
endif

call neobundle#begin(expand('$HOME/.config/nvim/bundle'))
NeoBundleFetch 'Shougo/neobundle.vim'

" ------------------------------------
" THIS IS WHERE YOUR PLUGINS WILL COME
" ------------------------------------
NeoBundle 'Valloric/YouCompleteMe', {
     \ 'build'      : {
        \ 'mac'     : './install.sh --clang-completer --system-libclang --omnisharp-completer',
        \ 'unix'    : './install.sh --clang-completer --system-libclang --omnisharp-completer',
        \ 'windows' : './install.sh --clang-completer --system-libclang --omnisharp-completer',
        \ 'cygwin'  : './install.sh --clang-completer --system-libclang --omnisharp-completer'
        \ }
     \ }
NeoBundle 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }
NeoBundle 'junegunn/fzf.vim'
NeoBundle 'neomake/neomake'
NeoBundle 'tomtom/tcomment_vim'
NeoBundle 'tpope/vim-surround'
NeoBundle 'jiangmiao/auto-pairs'
NeoBundle 'terryma/vim-multiple-cursors'
NeoBundle 'tpope/vim-fugitive'
NeoBundle 'godlygeek/tabular'
NeoBundle 'airblade/vim-gitgutter'
NeoBundle 'rhysd/clever-f.vim'
NeoBundle 'Shougo/vimproc.vim', {
\ 'build' : {
\     'windows' : 'tools\\update-dll-mingw',
\     'cygwin' : 'make -f make_cygwin.mak',
\     'mac' : 'make',
\     'linux' : 'make',
\     'unix' : 'gmake',
\    },
\ }
NeoBundle 'scrooloose/nerdtree'

NeoBundle 'pangloss/vim-javascript', { 'for': ['javascript', 'javascript.jsx'] }
NeoBundle 'mxw/vim-jsx', { 'for': ['javascript', 'javascript.jsx'] }

NeoBundle 'Quramy/tsuquyomi', { 'for': ['typescript', 'typescript.tsx'] }
NeoBundle 'HerringtonDarkholme/yats.vim', { 'for': ['typescript', 'typescript.tsx'] }
NeoBundle 'suzuki11109/vim-tsx', { 'for': ['typescript', 'typescript.tsx'] }


NeoBundle 'mfukar/robotframework-vim', { 'for': 'robot' }
NeoBundle 'ElmCast/elm-vim', { 'for': 'elm' }
NeoBundle 'fatih/vim-go', { 'for': 'go' }
NeoBundle 'othree/html5.vim', { 'for': ['html', 'html.mustache'] }
NeoBundle 'mustache/vim-mustache-handlebars', { 'for': 'html.mustache' }
NeoBundle 'mattn/emmet-vim', { 'for': ['html'] }
NeoBundle 'dag/vim2hs', { 'for': 'haskell' }
NeoBundle 'vim-ruby/vim-ruby', { 'for': 'ruby' }
NeoBundle 'JulesWang/css.vim', { 'for': 'css' }
NeoBundle 'cespare/vim-toml', { 'for': ['toml', 'markdown'] }
NeoBundle 'plasticboy/vim-markdown', { 'for': 'markdown' }

NeoBundle 'christoomey/vim-tmux-navigator'
NeoBundle 'tmux-plugins/vim-tmux-focus-events'
NeoBundle 'joshdick/onedark.vim'
NeoBundle 'editorconfig/editorconfig-vim'
call neobundle#end()

NeoBundleCheck
"End NeoBundle Scripts-------------------------

```

## 3. installl vimproc and YouCompleteMe
```
$cd bundle/vimproc
$make
$cd bundle/YouCompleteMe
$git submodule update --init —recursive
$./install.sh
```

## 4. add cofig to init.vim
```
" tsuquyomi
let g:tsuquyomi_auto_open = 0

"Neomake
autocmd! BufWritePost,BufReadPost * Neomake
let g:neomake_open_list=2
let g:neomake_list_height=5
let g:neomake_typescript_enabled_makers = []
let g:neomake_javascript_eslint_exe = 'eslint_d'
let g:neomake_javascript_enabled_makers = ['eslint']
let g:neomake_go_enabled_makers = ['go', 'govet']

"YCM
if !exists('g:ycm_semantic_triggers')
 let g:ycm_semantic_triggers = {}
endif
let g:ycm_semantic_triggers['elm'] = ['.']
let g:ycm_collect_identifiers_from_comments_and_strings = 1

" GitGutter
let g:gitgutter_sign_column_always = 1
let g:gitgutter_async = 0

" Emmet
let g:user_emmet_leader_key='<C-a>'

" Go
let g:go_highlight_functions = 1
let g:go_highlight_methods = 1
let g:go_highlight_structs = 1
let g:go_highlight_interfaces = 1
let g:go_highlight_operators = 1
let g:go_highlight_build_constraints = 1
let g:go_fmt_command = "goimports"
let g:go_fmt_fail_silently = 1
let g:go_doc_keywordprg_enabled = 0
autocmd FileType go set ts=4 sw=4 sts=4

"Markdown
let g:vim_markdown_toml_frontmatter = 1

"Elm
let g:elm_detailed_complete = 1
let g:elm_make_show_warnings = 1
let g:elm_setup_keybindings = 0

" FZF
let g:fzf_prefer_tmux = 1
if executable('fzf')
  let $FZF_DEFAULT_COMMAND = 'ag -l -g ""'
  function! s:find_root()
    for vcs in ['.git', '.svn', '.hg']
      let dir = finddir(vcs.'/..', ';')
      if !empty(dir)
        execute 'FZF' dir
        return
      endif
    endfor
    FZF
  endfunction

  command! FZFR call s:find_root()
  " <C-p> or <C-t> to search files
  nnoremap <silent> <C-p> :FZFR<cr>
  nnoremap <silent> <C-o> :FZF<cr>
  nnoremap <silent> <leader>bb :Buffers<cr>
end

" ======================================================================
" Config
" ======================================================================
set encoding=utf-8
set mouse=a

"Improve performance
set ttyfast

" Cursor motion
set backspace=indent,eol,start

" Allow hidden buffers
set hidden

" Use system clipboard
set clipboard=unnamed

set scrolloff=3

" Searching
set ignorecase
set smartcase
set incsearch
set showmatch
set hlsearch
set gdefault " use the `g` flag by default.

" Whitespace
set nowrap
set expandtab
set smarttab
set tabstop=2
set softtabstop=2
set shiftwidth=2
set shiftround
set autoindent
set smartindent

"show tabs and trail white space
set list lcs=tab:\|\ ,trail:·

"remove trail white space on save
autocmd BufWritePre * :%s/\s\+$//e

" We have vcs, we don't need backups.
set nobackup
set nowritebackup
set noswapfile

" Don't beep
set visualbell
set noerrorbells

set autoread

" disable folding
set nofoldenable

" disable comment in front of line
au FileType * set fo-=c fo-=r fo-=o

"=======================================================================
" Appearance
"=======================================================================
" show line number
set number

" Color
syntax on
set t_Co=256
set background=dark
let g:onedark_termcolors=16
colorscheme onedark

"custom color
hi MatchParen   cterm=bold
hi Visual       ctermbg=Gray ctermfg=0
hi ErrorMsg     ctermfg=Black ctermbg=Red cterm=bold
hi WarningMsg   ctermfg=Black ctermbg=Yellow cterm=bold
hi StatusLine   ctermbg=0 ctermfg=2
hi Normal guifg=#44cc44 guibg=NONE ctermbg=none

" Alwys status bar
set laststatus=2

" Last line
set showmode

" Disable preview on autocomplete
set completeopt-=preview

" show wild menu on statusline
set wildmenu

"Statusline
set statusline=\ %<%f
set statusline+=\ %m " modified
set statusline+=%#errormsg#

set statusline+=\%*
set statusline+=%="
set statusline+=%{fugitive#statusline()}
set statusline+=\ %y " file type
set statusline+=\ %l " line number
set statusline+=\ of
set statusline+=\ %L " line count

"=======================================================================
" Key bindings
"=======================================================================
"Map ; to : in normal model
nmap ; :

" Move up/down editor lines
nmap j gj
nmap k gk
nnoremap J 5j
nnoremap K 5k

"Clear search hightlights
nnoremap <leader><space> :noh<cr><esc>

"visual mode replace without yank
vnoremap p "_dP
nnoremap S "_S

vmap y y`]

"It remaps dd to "_dd if . (current line) is empty, or behave normally if the line is not empty.
nnoremap <expr> dd match(getline('.'), '^\s*$') == -1 ? 'dd' : '"_dd'

"Tab in normal mode
nnoremap <Tab> >>_
nnoremap <S-Tab> <<_
vnoremap <Tab> >gv
vnoremap <S-Tab> <gv
inoremap <S-Tab> <C-d>

nnoremap <silent> <BS> :TmuxNavigateLeft<cr>

"buffers
nmap <leader>bn :bn<cr>
nmap <leader>bp :bp<cr>
"====================================="
"-------------- NERDTree -------------"
"====================================="
let g:NERDTreeWinSize = 45
set wildignore+=*/tmp/*,*.so,*.swp,*.zip,*.pyc,*.db,*.sqlite
noremap <leader>n :NERDTreeToggle<CR>
noremap <leader>nf :NERDTreeFind<CR>

"=======================================================================
" Misc
"=======================================================================
"set OMR projects js file indent=4
autocmd BufNewFile,BufRead /Users/nong/Developments/omr/* setlocal ts=4 sw=4 sts=4

"set eslintrc as json
autocmd BufNewFile,BufRead .eslintrc setlocal filetype=json

"save last position
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif


```

### Done
