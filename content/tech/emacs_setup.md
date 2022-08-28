---
title: "Getting Started with Doom Emacs"
date: 2022-07-08
slug: "doom_emacs_intro"
description: "Tips and tricks for getting started with Doom Emacs"
keywords: ["doom", "doomemacs", "emacs", "setup", "guide", "tips", "tricks", "getting started"]
draft: false
tags: ["guide"]
math: false
toc: true
---

Are you tired of using a traditional editor? Looking for a break from the monotony... alongside a bit of Doom?

Look no further! [Doom Emacs](https://github.com/doomemacs/doomemacs) is a *configuration framework* for Emacs that abstracts away a lot of the complexity that goes into perfecting your setup, making it a much friendlier experience for beginners.
Doom is a purposely light layer over Emacs and emphasizes performance and startup time, without sacrificing extensibility.
It lets you configure Emacs just the way you like, while providing sane defaults and convenience.
Through Emacs' amazing packages like `Magit`, `Org` and `lsp`, you can really make the most of your editor!

The purpose of this post is to give you an intro to Doom/Emacs and give a brief overview of some of its features!

I'll be covering:
- What a typical workflow might look like + how to use Emacs.
- How to configure Doom using its built-in module system.
- My recommendations for packages, config tweaks + how I like to use Emacs.

Before getting started, I'd highly recommend getting (at least) somewhat familiar with `vim` keybindings (try `vimtutor` by installing [Neovim](https://neovim.io/) and running `nvim +Tutor`).

## Installation

Installing Emacs/Doom is very OS specific, so I'm just going to defer to the [doomemacs docs](https://github.com/doomemacs/doomemacs#install) here (if you're on Windows just use `WSL`).

> A small note about Emacs versions, `emacs28` (the latest Emacs version at the time of writing) introduces the `native compilation` feature, which speeds up the execution of `Elisp`, Emacs' lisp variant. So it's an all around Good Thing™️.

When everything is set up, run Emacs and you should be greeted to a splash screen like this:

<img src="/img/doom_startup.jpg" alt="startup" style="zoom:200%;" />

## First Moments

So, now you're in Emacs, congratulations! But, how do you *do* anything? Here are some helpful keybindings to help you navigate around and start working with files. 

| Keybinding  | Description                  |
|-------------|------------------------------|
| `<SPC> .`   | Find file                    |
| `<SPC> ,`   | Switch buffer in a workspace |
| `<SPC> b k` | Kill buffer                  |

So, run `<SPC> .` and open a file of your choice.

If you open another file and would like to navigate back, you can switch the buffer that you're looking at with `<SPC> ,`.
If you feel that your buffer list is too crowded, or that you're fully done with working on the current file, use `<SPC> b k` to kill the buffer and remove it from the list.

### Projects/Workspaces

I typically structure my business into specific directories, so it'd be helpful if Emacs let me work on projects like a typical editor. Well... it's Emacs, so of course you can. Here are some helpful project-related keybindings.

| Keybinding    | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| `<SPC> p a`   | Add known project                                                           |
| `<SPC> p p`   | Switch project                                                              |
| `<SPC> <SPC>` | Find file in project                                                        |
| `<SPC> /`     | Project wide search (with [ripgrep](https://github.com/BurntSushi/ripgrep)) |

*NOTE*: For `projectile` to recognize a "project", it must contain a [special file](https://docs.projectile.mx/projectile/index.html#features) like a `.git` or `.projectile`. If it does not contain one, then it will silently remove your directory from the list.

### Getting help + Discovery

You might've heard that Emacs is self-documenting, but how do we actually take advantage of that?

Well, there are handy commands for learning more about the functions we're using in addition to discovering new ones. Type `<SPC> h` and wait for a moment to view the possible commands you can run for getting help.

<img src="/img/which_key.jpg" alt="which-key" style="zoom:200%;" />

Most of these are a little too involved for now, but I'll be covering the hits (You can also access these via `C-h`).


| Keybinding  | Description                                      |
|-------------|--------------------------------------------------|
| `M-x ...`   | Search for and run any interactive function      |
| `<SPC> h k` | Describes a command associated with a keybinding |
| `<SPC> h f` | Describes a function                             |
| `<SPC> h v` | Describes a variable                             |


`M-x` (`Alt x`) is a very commonly used keybinding.
It lists every command you have access to. You typically won't be using many of the commands in this list,
but it's useful for learning more about what a package can do.
For example, try searching for `projectile` to get an idea of the commands you have access to (sometimes without keybindings), such as `projectile-replace` for `find and replace` functionality.

For describing/ learning more about commands (examples, docs etc...), try `<SPC> h k` and typing `<SPC> .` to learn more about the `find-file` function.
Next, try looking at a description for the `doom-font` variable with `<SPC> h v`. 
Here you'll see documentation about how `Doom` configures its font. Which makes for a good segue about learning about how to configure Doom.

## Understanding Your Config

| Keybinding    | Description                                             |
|---------------|---------------------------------------------------------|
| `<SPC> f p`   | Find file in private config                             |
| `<SPC> h r r` | Reload your config from within Emacs (like `doom sync`) |
| `<SPC> q r`   | Restart Emacs and restore the current session           |
| `<SPC> h t`   | Choose a theme                                          |

Located in `$DOOMDIR`, Doom's configuration is mainly controlled by three files: `init.el`, `config.el`, `packages.el`.
To learn more about what each of these files do, I *highly* recommend reading through [this section](https://github.com/doomemacs/doomemacs/blob/develop/docs/getting_started.org#configure) in the doom docs.

### `init.el`

`init.el` contains your `doom!` block, which is how you'll be configuring which packages/modules Doom installs. To get an overview of how to work with it, here's the [relevant section](https://github.com/doomemacs/doomemacs/blob/develop/docs/getting_started.org#modules) in the docs about modules. 

Each module has built-in documentation that you can easily by typing `K` with your cursor over the module. As of the time of writing, the docs are still a work in progress, but you can still find helpful information about module flags and packages that a module installs. 

<img src="/img/module_docs.jpg" alt="module-docs" style="zoom:200%;" />

When looking into installing additional packages from guides online, you'll first want to check whether there's a relevant module in `init.el` that you can activate instead because modules are pre-configured and are well integrated into Doom.

If you've got some time on your hands, you can window-shop through `init.el` and tweak it to your liking. I'll also be going over my recommendations over in [Turn Emacs into an IDE](#turn-emacs-into-an-ide).

To have your changes take effect, you can run `doom sync` or `<SPC> h r r` from within Emacs. 

### `config.el`

`config.el` is where you configure (shocker!) the packages you install in `init.el` and `packages.el`. Emacs is built on `Elisp` (an Emacs-specific lisp variant), so your configuration will also be written in Elisp.
However, you can get pretty far with just a basic understanding of Elisp.

Here's a rudimentary rundown:

#### Basic Elisp


```elisp
;; (setq key val)

;; Basic vales
(setq my-var 10)          ;; number
(setq my-name "Aquabeam") ;; string
(setq my-name 'Aquabeam)  ;; symbol

;;; multiple items in one setq
(setq item1 10
      item2 20
      item3 30)

;; Lists
(setq my-list '(1 2 3 4)) ;; lists of numbers

;; Booleans
(setq item t)   ;; true
(setq item nil) ;; false

;; Functions
(message "hello")
```

You can get a **LOT** deeper into `Elisp`, but this is good enough for now.

Here's a demo showcasing some basic configuration options:

#### Basic configuration

- [Configure font](https://github.com/doomemacs/doomemacs/blob/develop/docs/faq.org#how-do-i-change-the-fonts)

```elisp
(setq doom-font (font-spec :family "Jetbrains Mono" :size 13.5))
```

- [Configure theme](https://github.com/doomemacs/doomemacs/blob/develop/docs/faq.org#how-do-i-change-the-theme)

Use `<SPC> h t` to view/demo themes. Also take a look at [doomemeacs/themes](https://github.com/doomemacs/themes) for screenshots.

```elisp
(setq doom-theme `doom-old-hope)
```

### `packages.el`

This is where you install packages that aren't built into Doom's modules. Before installing anything, give [the relevant section](https://github.com/doomemacs/doomemacs/blob/develop/docs/getting_started.org#package-management) in the docs a read, it contains warnings + good advice.


#### Quick packages overview:

In short:

- If a package is available on one of Emacs's package repositories (MELPA, ELPA etc...):

```elisp
(package! your-package)
```

- If not, you can specify from where to install the package.

Here's an example with a package that offers excellent high contrast and accessible themes: prot's [`ef-themes`](https://protesilaos.com/emacs/ef-themes).

```elisp
(package! ef-themes :recipe
  (:host github
   :repo "protesilaos/ef-themes"
   :branch "main")) ;; really pretty themes
```

To have your changes take effect, you can run `doom sync` or `<SPC> h r r` from within Emacs. 

## Turn Emacs into an IDE

⚠️  ⚠️   IN PROGRESS ⚠️  ⚠️

### Languages

### `lsp` - VSCode-esque intellisense 

### `vterm`- a sweet terminal

### `treesitter`- modern syntax highlighting



## Very-Good™️  packages

### Org - plaintext, but better

#### Making Org Prettier

#### Org-babel - execute code blocks in a file


### Magit - a sane Git interface

### Treemacs - view your files in the sidebar


## General tips

### Managing windows

### Workspaces

### Emacs daemon + tty: in and out quickly

  
  
  
