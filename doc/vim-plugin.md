currentMenu: vim-plugin
---
---
Phpactor VIM Plugin
===================

- [Installation](#installation)
- [Updating](#updating)
- [Configuration](#configuration)
- [Completion](#completion)
- [Completion plugins](#completion-plugins)
- [Context Menu](#context-menu)

Installation
------------

Install Phpactor using your favorite VIM package manager, I am using Vundle.
Add the plugin to your `.vimrc`:

```
Plugin 'phpactor/phpactor'
```

Then in VIM:

```
:VundleInstall
```

Now you will need to install the Phpactor dependencies with composer:

```
$ cd ~/.vim/bundle/phpactor
$ composer install
```

or using nvim, do the above where it counts.

<div class="alert alert-info">
Make a <b><i class="fa fa-github"></i> <a href="https://github.com/phpactor/phpactor">Pull Request</a></b> to improve this
installation procedure!
</div>

Now issue the following command `:call phpactor#Status()`:

```
Support
-------
[✔] Composer detected - faster class location and more features!
[✔] Git detected - enables faster refactorings in your repository scope!

Config files
------------
[✔] /home/daniel/www/phpactor/phpactor/.phpactor.yml
[✔] /home/daniel/.config/phpactor/phpactor.yml
[✘] /etc/xdg/phpactor/phpactor.yml

```

Phpactor works best with Composer - but much functionality including
auto-completion can still work (sometimes slowly depending on project size).

Updating
--------

Updating Phpactor from VIM is easy:

```vim
:call phpactor#Update()
```

<div class="alert alert-warning">
Note that if the update included changes to the VIM plugin you will currently
need to either re-source (`:source ~/path/to/phpactor/plugin/phpactor.vim`) the plugin or reload VIM (pull requests are open!).
</div>

If you are feeling dangerous, you may choose to track the `develop` branch,
by specifying a branch name in your `.vimrc`:

Keyboard Mappings
-----------------

The Phpactor plugin will **not** automatically assume any shortcuts, copy
the following configuration into your `.vimrc`:

```vimscript
" Include use statement
nmap <Leader>u :call phpactor#UseAdd()<CR>

" Invoke the context menu
nmap <Leader>mm :call phpactor#ContextMenu()<CR>

" Goto definition of class or class member under the cursor
nmap <Leader>o :call phpactor#GotoDefinition()<CR>

" Transform the classes in the current file
nmap <Leader>tt :call phpactor#Transform()<CR>

" Generate a new class (replacing the current file)
nmap <Leader>cc :call phpactor#ClassNew()<CR>

" Extract method from selection
vmap <silent><Leader>em :<C-U>call phpactor#ExtractMethod()<CR>
```

See the [Refactorings](refactorings.md) chapter for more functions you can map
shortcuts to.

Phpactor requires at least PHP 7.0. If you use a different version of PHP
locally, you may need to target a new version of PHP - add the following to
your `.vimrc` to change the PHP binary:

```
let g:phpactorPhpBin = "/usr/bin/local/php6.0"
```

Configuration
-------------

The plugin has some configuration options:

```
let g:phpactorPhpBin = 'php'
let g:phpactorBranch = 'master'
let g:phpactorOmniError = v:false
```

- `g:phpactorPhpBin`: PHP executable to use.
- `g:phpactorBranch`: Phpactor branch (default is `master`, use `develop` for
  bleeding edge).
- `g:phpactorOmniError`: Set to `v:true` to enable useful error messages when
  completion is invoked.

Completion
----------

### Omni-completion

Omni-completion
([Screenshot](./screenshots.html#code-completion)) is
VIM's built-in auto-completion mechanism.

Add the following to your `.vimrc` in order to use Phpactor for omni-completion (for PHP files):

```vimscript
autocmd FileType php setlocal omnifunc=phpactor#Complete
```

To invoke omni complete in insert mode `<C-x><C-o>` (`ctrl-x` then `ctrl-o`).
See `:help compl-omni`.

<div class="alert alert-info">
<p>
The Omni-Complete method provides <i>feedback</i> messages when it cannot complete something. This information
is <b>useful</b>. Other completion mehanisms may not provide this information.
</p>
<p>
<b>Always enable omni-completion</b>. If another mechanism is failing to complete, invoke omni-complete to find out why.
</p>
</div>

Completion plugins
------------
Several asynchronous completion frameworks support Phpactor

### Deoplete
To use Phpactor with [deoplete.nvim](https://github.com/Shougo/deoplete.nvim) add this to your vimrc/init.vim:

*Using [vim-plug](https://github.com/junegunn/vim-plug)*
```vimL
Plug 'Shougo/deoplete.nvim'
Plug 'phpactor/phpactor', { 'do': 'composer install', 'for': ['php'] }
Plug 'kristijanhusak/deoplete-phpactor'
```

+Run `PlugInstall` and you're good to go!

### Neovim Completion Manager
To use Phpactor with [Neovim Completion Manager](https://github.com/roxma/nvim-completion-manager) add this to your vimrc/init.vim:

*Using [vim-plug](https://github.com/junegunn/vim-plug)*
```vimL
Plug 'roxma/nvim-completion-manager'
Plug 'phpactor/phpactor', { 'do': 'composer install', 'for': ['php'] }
Plug 'roxma/ncm-phpactor'
```

+Run `PlugInstall` and you're good to go!

Context Menu
------------

The context menu is the main point of contact with Phpactor. Invoke it on any
class, member, variable, method call, or anything really.

If you move over a method and invoke the context menu with `:call
phpactor#ContextMenu()` (or with `<Leader>mm` as per the configuration above) you
should see something like the following:

```
Method "execute":
[r]eplace_references, (f)ind_references, (g)enerate_method, g(o)to_definition:
```
