---
title: "Getting Started with Doom Emacs"
date: 2022-09-04
slug: "doom_emacs_intro"
description: "Tips and tricks for getting started with Doom Emacs"
keywords: ["doom", "doomemacs", "emacs", "setup", "guide", "tips", "tricks", "getting started"]
draft: false
tags: ["guide"]
showTableOfContents: true
---

Are you tired of using a traditional editor?
Looking for a break from the monotony... plus a little bit of Doom?

Look no further! [Doom Emacs](https://github.com/doomemacs/doomemacs) is a *configuration framework* for Emacs that abstracts away a lot of the complexity that goes into perfecting your setup, making it a much friendlier experience overall.
Doom is a purposely light layer over Emacs that emphasizes performance and startup time, without sacrificing extensibility.
It also provides (optional) vim emulation powered by [`evil-mode`](https://github.com/emacs-evil/evil) out of the box, so if you're comfortable with vim keybindings, you'll feel right at home.
With Emacs' vast selection of amazing packages and excellent language server integration, you can make the most of your editor!

----

The goal of this post is to give you an intro to Doom/Emacs and offer an overview of some of its features.

I'll be covering:
- What a typical workflow might look like.
- How to configure Doom using its built-in module system.
- My recommendations for packages and config tweaks: [Turn Emacs into an IDE](#turn-emacs-into-an-ide).

Before getting started, I'd highly recommend getting (at least) somewhat familiar with `vim` keybindings (try `vimtutor` by installing [Neovim](https://neovim.io/) and running `nvim +Tutor`).

## Installation

Installing Emacs/Doom is very OS-specific, so I'm just going to defer to the [doomemacs docs](https://github.com/doomemacs/doomemacs#prerequisites) here (if you're on Windows just use `WSL`).

{{< alert "edit" >}}
A small note about Emacs versions, `emacs28` (the latest Emacs version at the time of writing) introduces the `native compilation` feature, which speeds up the execution of Elisp, Emacs' Lisp variant.
So it's an all-around Good Thing™️.
{{< /alert >}}

When everything is set up, run Emacs and you should be greeted by a splash screen like this:

![initial Doom view](/img/doom_startup.jpg "Initial Doom Emacs View")

## First Moments

So, now you're in Emacs, congratulations! But, how do you *do* anything? Here are some helpful keybindings to help you navigate around and start working with files. 


| Keybinding         | Description                  |
|--------------------|------------------------------|
| <kbd>SPC .</kbd>   | Find file                    |
| <kbd>SPC ,</kbd>   | Switch buffer in a workspace |
| <kbd>SPC b k</kbd> | Kill buffer                  |

So, run <kbd>SPC .</kbd> and open a file of your choice.

If you open another file and would like to navigate back, you can switch the buffer that you're looking at with <kbd>SPC ,</kbd>.
If you feel that your buffer list is too crowded, or that you're fully done with working on the current file, use  <kbd>SPC b k</kbd>  to kill the buffer and remove it from the list.

### Projects

I typically structure my business into specific directories, so it'd be helpful if Emacs let you work on projects like a typical editor.
Well... it's Emacs, so of course you can! Here are some helpful project-related keybindings.


| Keybinding         | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| <kbd>SPC p a</kbd> | Add known project                                                           |
| <kbd>SPC p p</kbd> | Switch project                                                              |
| <kbd>SPC SPC</kbd> | Find file in project                                                        |
| <kbd>SPC /</kbd>   | Project wide search (with [ripgrep](https://github.com/BurntSushi/ripgrep)) |

{{< alert  >}}
**Warning!** For `projectile` to recognize a "project", it must contain a [special file](https://docs.projectile.mx/projectile/index.html#features) like a `.git` or `.projectile`. If it does not contain one, then it will silently remove your directory from the list.
{{< /alert >}}

### Getting help + Discovery

You might've heard that Emacs is self-documenting, but how do we actually take advantage of that?

Well, there are handy commands for learning more about the functions we're using on top of discovering new ones.
Type <kbd>SPC h</kbd> and wait for a moment to view the available help commands and their corresponding keybinds.

![](/img/which_key.jpg "See possible keybindings with `which-key`")

Most of these are a little too involved for now, but I'll be covering the hits (You can also access these via <kbd>C-h</kbd>).


| Keybinding           | Description                                      |
|----------------------|--------------------------------------------------|
| <kbd>M-x ...</kbd>   | Search for and run any interactive function      |
| <kbd>SPC h b b</kbd> | Explore all available key bindings               |
| <kbd>SPC h k</kbd>   | Describes a command associated with a keybinding |
| <kbd>SPC h f</kbd>   | Describes a function                             |
| <kbd>SPC h v</kbd>   | Describes a variable                             |


<kbd>M-x</kbd> (<kbd>Alt x</kbd>) is a very commonly used keybinding.
It lists and allows you to run every command you have access to.
You typically won't be using many of the commands in this list, but it's useful for learning more about what a package can do.
For example, try searching for `projectile` to get an idea of the commands you have access to (sometimes without keybindings), such as `projectile-replace` for *find and replace* functionality.

In the same spirit as <kbd>M-x</kbd>, <kbd>SPC h b b</kbd> lets you explore the keybindings you have access to.
You can try searching for either a specific keybind or package name.

For describing / learning more about commands (examples, docs etc...), try <kbd>SPC h k</kbd> and typing <kbd>SPC .</kbd> to learn more about the `find-file` function.

Next, try looking at the description for the `doom-font` variable with <kbd>SPC h v</kbd>. 
Here you'll see documentation and examples about how Doom configures your font. Which makes for a good segue about learning how to configure Doom Emacs!

## Understanding Your Config

| Keybinding           | Description                                             |
|----------------------|---------------------------------------------------------|
| <kbd>SPC f p</kbd>   | Find file in private config                             |
| <kbd>SPC h r r</kbd> | Reload your config from within Emacs (like `doom sync`) |
| <kbd>SPC q r</kbd>   | Restart Emacs and restore the current session           |
| <kbd>SPC h t</kbd>   | Choose a theme                                          |

Located in `$DOOMDIR`, Doom's configuration is mainly controlled by three files: `init.el`, `config.el` and `packages.el`.
To learn more about what each of these files do, I *highly* recommend reading through [this section](https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org#configure) in the Doom docs.

### `init.el`

`init.el` contains your `doom!` block, which is how you'll be configuring which packages/modules Doom installs.
To understand how it works, read through the [module section](https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org#modules) in the docs. 

Each module has built-in documentation that you can easily view by typing <kbd>K</kbd> with your cursor over the module.
As of the time of writing, the docs are still a work in progress, but you can still find helpful information about module flags and packages that a module installs. 

![](/img/module_docs.jpg "Module docs for Rust")

If you're looking into installing additional packages from online guides, you'll first want to check whether there's a relevant module in `init.el` that you can activate instead because modules are pre-configured and are well integrated into Doom.

If you've got time on your hands, you can window-shop through `init.el` and tweak it to your liking.
I'll also be going over the highlights in [Turn Emacs into an IDE](#turn-emacs-into-an-ide).

To have your changes take effect after modifying your `init.el`, you can run `doom sync` in your terminal or <kbd>SPC h r r</kbd> from within Emacs. 

### `config.el`

`config.el` is where you configure (shocker!) the packages you install in `init.el` and `packages.el`.
Emacs is built on Elisp (an Emacs-specific Lisp variant), so your configuration will also be written in Elisp.

If you're not comfortable with Lisp, don't worry!
You can get pretty far with just a basic understanding of the language.
Here's a rudimentary rundown:

#### Basic Elisp

```elisp
;; (setq key val)

;; Basic values
(setq my-var 10)          ;; number
(setq my-name "Aquabeam") ;; string
(setq my-name 'Aquabeam)  ;; symbol

;; Multiple items in one setq
(setq item1 10
      item2 20
      item3 30)

;; Lists
(setq my-list '(1 2 3 4)) ;; list of numbers

;; Booleans
(setq item t)   ;; true
(setq item nil) ;; false

;; Functions
(message "hello")
```

You can get a **LOT** deeper into Elisp, but this is good enough for now.
Here's a demo showcasing some basic configuration options:

- [Configure font](https://github.com/doomemacs/doomemacs/blob/master/docs/faq.org#how-do-i-change-the-fonts)

```elisp
(setq doom-font (font-spec :family "Jetbrains Mono" :size 13.5))
```

- [Configure theme](https://github.com/doomemacs/doomemacs/blob/master/docs/faq.org#how-do-i-change-the-theme)

```elisp
(setq doom-theme `doom-old-hope)
```

Use <kbd>SPC h t</kbd> to try out available themes.
Also, take a look at [doomemeacs/themes](https://github.com/doomemacs/themes) for screenshots.


#### More advanced configuration

```elisp
(use-package! markdown-mode
  :defer t
  :config
  (setq markdown-fontify-code-blocks-natively t)
  )
```

- `use-package!` is a macro for grouping together related configurations. It helps keep your `config.el` structured as it grows. 
- `:defer` is a property which delays loading the package until needed (i.e. when you open a markdown file), which helps improve startup time by not loading packages you might not use.

{{< alert >}}
**Caution!** When writing your own config, make sure to always use either `:after PACKAGE` or `:defer t` in `use-package!` blocks to lazy load your packages.
{{< /alert >}}

### `packages.el`

This is where you install packages that aren't built into Doom's modules. Before installing anything, give [this section](https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org#package-management) in the docs a read, it contains warnings + good advice.


#### Quick packages overview

In short:

- If a package is available on one of Emacs's package repositories (MELPA, ELPA etc...):

```elisp
(package! your-package)
```

- If not, you can specify where to install the package.

Here's an example with a package that offers excellent high contrast and accessible themes: prot's [`ef-themes`](https://protesilaos.com/emacs/ef-themes).

```elisp
(package! ef-themes :recipe
  (:host github
   :repo "protesilaos/ef-themes"
   :branch "main")) ;; really pretty themes
```

To have your changes take effect, you can run `doom sync` or <kbd>SPC h r r</kbd> from within Emacs. 

## Turn Emacs into an IDE

If you're coming over from another editor or IDE, you might be wondering how to make Emacs do all the fancy things your previous editor could do, or if it's even possible.
Fortunately, it is not only possible but also quite painless to get up and running.
Thanks to Doom's great defaults, all you have to do is toggle some modules in your `init.el` and carry on like usual!

### Working with languages

To get all the configuration, packages and conveniences you'd expect for a programming language of your choice, simply head over to the `:lang` section of your `init.el` and un-comment the language.
After that, make sure to reload your private config to make your changes take effect.
Some modules also have additional flags that you can enable for additional language-specific features. 

For instance, I enable `javascript` integration with the following line in my `init.el`

```lisp
(javascript +lsp +tree-sitter) ; all(hope(abandon(ye(who(enter(here))))))
```

These flags in particular (`lsp` + `tree-sitter`) are very important and applicable to many languages!
I'll be covering them in the following sections.


{{< alert "edit" >}}
You can find out which flags are available by typing <kbd>K</kbd> with your cursor over a module.
{{< /alert >}}

### `lsp` - VSCode-esque intellisense 

The `lsp` module integrates [language servers](https://langserver.org/) into Emacs, providing features like jump-to-definition, code completion, real-time linting and more!
To get started, enable the `lsp` module under `:tools` which will configure and install [`lsp-mode`](https://emacs-lsp.github.io/lsp-mode/). 

Then, for each language where you'd like to take advantage of `lsp` integration, just add the `+lsp` flag to its module.
If you do not have an appropriate language server installed, `lsp-mode` will prompt you to install it on your behalf. 

When first opening a file that supports `lsp`, you'll see the following message:

![](/img/lsp-mode-first.jpg "Initial LSP-mode view")

The first option to import the project root (<kbd>i</kbd>) is probably the one you want.
Although, if the project root doesn't look right, then you might want to set it yourself with <kbd>I</kbd>.

{{< alert >}}
**Warning!** Avoid setting extremely large directories (like your home dir) as the project root. Since `lsp-mode` will then watch every file in the directory, slowing Emacs down. 
{{< /alert >}}

Here are some neat things you can do in `lsp-mode` (LSP keybindings are grouped together under  <kbd>SPC c ...</kbd>):

| Keybinding                      | Description                                                                |
|---------------------------------|----------------------------------------------------------------------------|
| <kbd>gd</kbd>                   | Go to definition (use <kbd>C-o</kbd> to return, <kbd>C-i</kbd> to go back) |
| <kbd>gD</kbd>                   | Find all references of an item under your cursor                           |
| <kbd>SPC c r</kbd>              | Rename an item (context aware) under your cursor                           |
| <kbd>SPC c k</kbd>/<kbd>K</kbd> | View documentation of an item under your cursor                            |


### Tree-sitter - modern syntax highlighting

The built-in Emacs syntax highlighting for some languages can be lacking at times.
Fortunately, thanks to Tree-sitter there's a way to improve it.

I'll defer to the [`emacs-tree-sitter`](https://emacs-tree-sitter.github.io/) package for an explanation.
Essentially, Tree-sitter introduces more performant and robust syntax highlighting which gives you more control over the buffer.
Not all languages currently have support for tree-sitter parsing (refer to the module docs for more info).
However, you'll definitely want to use it when available.

To enable Tree-sitter support for a language, just add the `+tree-sitter` flag to the module.

### `vterm`- a sweet terminal

Every good editor needs an even better terminal. Emacs has a few solutions for built-in terminals, but `vterm` is the clear winner.
Since it's based on `libvterm` (an external C library), it's fast, light and offers "near universal compatibility with terminal applications" (according to the [project repository](https://github.com/akermu/emacs-libvterm)).

To enable it, activate the `vterm` module in your `init.el`.
However, since it requires an external dependency, you'll be prompted to build `vterm-module.so` during the installation process. 
Refer to the [module docs](https://docs.doomemacs.org/latest/modules/term/vterm/) if anything goes wrong.

Here are the keybindings you need to know about:

| Keybinding         | Description                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------------|
| <kbd>SPC o t</kbd> | Open a terminal buffer                                                                            |
| <kbd>SPC o T</kbd> | Open a terminal buffer in your current window <br> (allows having multiple open terminal buffers) |

### Treemacs - view your files in the sidebar

Having a project drawer is a very convenient tool for managing files and understanding your project's directory structure.
Thankfully Doom provides the `treemacs` module which does just that.
Simply enable `treemacs` in your `init.el` (also consider the `+lsp` flag for some additional `lsp-mode` integration)

<!-- have to do it like this because the image is too wide, and the figure shortcode won't accept width as a parameter -->
<!-- the class makes it rounded here -->
<!-- TODO: figure out how to center -->
<figure>
    <img
    src="/img/treemacs-view.jpg" 
    class="my-0 rounded-md"
    alt="A file viewer in Emacs" 
    width=250 />
  <figcaption>Treemacs Sidebar</figcaption>
</figure>

I also have some additional configuration in my `config.el` to improve its appearance.

```elisp
(use-package! treemacs
  :defer t
  :config
  ;; alters file icons to be more vscode-esque (better 😼)
  ;; https://github.com/doomemacs/themes/wiki/Extension:-Treemacs
  (setq doom-themes-treemacs-theme "doom-colors")
  )
```

Here's an overview of the keybinds I use most frequently:

| Keybinding         | Description                                      |
|--------------------|--------------------------------------------------|
| <kbd>SPC o p</kbd> | Initialize/toggle treemacs                       |
| <kbd>SPC o P</kbd> | Focus the current file in the treemacs window    |
| <kbd>cf</kbd>      | Create a file                                    |
| <kbd>cd</kbd>      | Create a directory                               |
| <kbd>R</kbd>       | Rename a file/dir                                |
| <kbd>m</kbd>       | Move a file/dir                                  |
| <kbd> < </kbd>     | Decrease width of side pane                      |
| <kbd> > </kbd>     | Increase width of side pane                      |
| <kbd>M-H</kbd>     | Move the project root one level up               |
| <kbd>M-L</kbd>     | Move the project root one level down (at cursor) |

### Magit - a sane Git interface

Despite the ubiquity of `Git`, it still carries a reputation for being difficult to use and understand. 
So much so that if something ever goes wrong, it's quite compelling to just give up, blow up your repo and start from scratch.

[Magit](https://magit.vc/ "Magit website") changes that.

After getting comfortable with Magit, I've noticed a huge difference in the way I think about organizing commits and my repos. Magit taught me how to make the most of `Git`, not just as a means to an end.

Flanerie aside... Magit is great! There's a reason it's known as one of "Emacs Killer Apps". The hype is real.

As expected, just enable the `magit` module in your `init.el` to install the package.
Unfortunately, it's a little difficult to understand how to use Magit from a table of keybindings, so I'll be deferring to SystemCrafers' excellent Magit introduction video here. It goes through all you need to know to get started with Magit!

{{< youtube  _zfvQkJsYwI >}}

## General tips

Here's a collection of information that's neat to know. 

### Managing windows

If you're particularly adept at multitasking, you might enjoy being able to look at multiple buffers at once.
If so, here are the keybindings you need to know. Most of these can be repeated by typing <kbd>.</kbd>, so you won't need to retype the entire command when making small modifications.


| Keybinding            | Description                                               |
|-----------------------|-----------------------------------------------------------|
| <kbd>C-x }</kbd>      | Enlarge window horizontally                               |
| <kbd>C-x {</kbd>      | Shrink window horizontally                                |
| <kbd>C-x ^</kbd>      | Enlarge window vertically                                 |
| <kbd>C-w v</kbd>      | Split window vertically                                   |
| <kbd>C-w s</kbd>      | Split window horizontally                                 |
| <kbd>C-w m m</kbd>    | Maximize current buffer                                   |
| <kbd>C-w [hjkl]</kbd> | Navigate around open windows (just like regular movement) |

There are doom keybindings for these commands under the <kbd>SPC w</kbd> prefix, but I personally find the evil-mode defaults more ergonomic.

![](/img/multi-window.jpg.jpg "Multiple windows in Emacs")

### Workspaces

Workspaces are a way of keeping different projects distinctly separate.
For example, they enable you to draft notes and churn out some code in the same Emacs instance, with no overlap between buffers/terminals/sidebars at all.
You might also notice that when you switch to a new project with <kbd>SPC p p</kbd>, a new workspace is automatically created for you.

Here are the keybinds you need to know (mostly grouped under <kbd>SPC TAB</kbd> ):

| Keybinding             | Description                            |
|------------------------|----------------------------------------|
| <kbd>gt</kbd>          | Go to the next workspace               |
| <kbd>SPC TAB n</kbd>   | Create a new workspace                 |
| <kbd>SPC TAB N</kbd>   | Create a new named workspace           |
| <kbd>SPC TAB d</kbd>   | Delete active workspace                |
| <kbd>SPC TAB r</kbd>   | Rename active workspace                |
| <kbd>SPC TAB TAB</kbd> | Display active workspaces in echo area |

