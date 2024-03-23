# The Rustifyer dotfiles configuration

Hi there everyone! This is my GitHub's repository for storing my more beloved configuration on certain
system or applications.

This allows me to quickly setup my full development environment (as I like to have it) just by cloning and pulling
this repository in any machine.

Also, I can share and have up-to-date all my configuration along my typically used machines.

## Installation process

Typically, in my daily basis I use a **Linux** distro (native) and a **Windows**, so I always liked the idea of using
the same tools, regarding which operating system I am using at a particular moment. But that's a hard thing to acomplish,
because I also tend to be happier using community-driver open source tools (or at least, open source), and the tools have
to be crossplatform by default or at least, be available in runtimes like `Cygwin` or `Msys2`, or at least, natively compilable
in `Windows` via tools like `Mingw`.

The fact is that, all the tools presented below are easy to get, or they already come with your Linux installation. So simply.
Yet `Windows` is another thing.

## Previous considerations:

>[!WARNING]
>
> This documentation assumes that `Linux` distros will be any kind of `Arch` variants, or that they use (have installed)
> `Pacman` in any other `Linux` distro.

>[!CAUTION]
>
> Note for myself. Remember to check in the bash script for the presence of `pacman` in the system.
> If not present, just install it, so we can make more kind of distros compatibles with this *setup*


>[!CAUTION]
>
> Note for myself. Remember to create `Github` actions to check that the packages are installable without problem in
> all the supported OS

## The key in Windows, [MSYS2](https://www.msys2.org/)

>[!TIP]
>
> If you plan to use this guide for install the *setup* only on `Linux`, just skip this [TODO, change skip this for a link]

**MSYS2** is a collection of tools and libraries providing you with an easy-to-use environment for building, installing and running native Windows software.

For more details see ['What is MSYS2?'](https://www.msys2.org/docs/what-is-msys2/)

So basically, the first step to get everything running in Windows is to download the installer. So click on the hyperlink, download it
and then double click on your installer.

>[!NOTE]
>
> Installation extremely fast-forward process, the unique doubt could be the installation location. `MSYS2` is typically well placed directly
> under `C:\`. You can install it in other place, but this guide assumes that you will be using `C:\msys64`, as recommended in their documentation.

You'll see the in latest slide of the installation prompt a ticked checkbox asking you for execute `msys2` now. Discard it and close the installation prompt.

Now, on the `Windows` search bar, type msys2, and open the "purple" shell (msys2 msys), and type:

```bash
pacman -Syu
```

Obviously, type **[Y]** and hit `Enter`. This will update your `msys2` subsystem with the latest packages.

### The `clang64` enviroment.

Now, type `clang64` on the Windows search bar, and open such prompt. Then, run again `pacman -Syu`.

You can learn more about this `msys2` enviroment and more about all of them [here](https://www.msys2.org/docs/environments/). I'll recommend you
to take your time to carefully read about them. Isn't an easy thing to grasp at first if you're not experienced in this kind of complex setups, 
so take a deep breath and just **read the docs**. // TODO pls insert meme here

The very key thing about here is that you'll have all the coolest environment configuration available to work with the `llvm-suite` compiler tech and build
amazing software with them. That's not exactly the whole point, but once you taste it, you'll won't change it.

### Git

There's now two different options from here.

1. Install the `Git for Windows` project. This uses a custom `msys2` self-contained `msys2` environment, slightly different from the upstream one. But it has some advantages, like a GUI installer, based on *click, accept, next* and it is ready to go. It is nice, and if you don't want to complicate thing you may consider it as you option.
2. Using the `msys2` git installation. Since we already have a `msys2` installation, and you don't care about performance, you could use the already embeed git (posix compliant) git installation on the `msys2 msys` base environment. But it has its flaws. Since is in the *msys* environemnt, it works on the *POSIX* emulation layer, using the `msys` runtime tools and shared libraries. This has the downside that it will introduce overheat to the git binary tools when you invoke it, specially noticeable when you work with medium to large *git* based projects.
3. Use the `mingw` based *git* installation. This is far the most complicated one to set up, but it has the best of both worlds. It is performant (since is natively compiled for *Windows*) and you can't avoid to install yet another `git` tool on your system, polluting your development environment with more and more binaries, potentially running into **PATH** issues when invoking tools and other stuff. This guide will use this approach, but feel free to use the one that better suits your purposes.

#### [Install GIT inside MSYS2 proper](https://github.com/git-for-windows/git/wiki/Install-inside-MSYS2-proper)

Instead of describing the steps here, just follow the hyperlink on the header above. They will have the latest known working version to efficiently install
**Git** inside **MSYS2** properly, and I don't want to update the instructions when it changes. Is not exactly easy and you can run into some issues (even they are unlikely) so I warn you to read first all the documentation and then go again from the beggining, so you can jugde by yourself if this method really suits you.

>[!TIP]
>
> Just remember to open the `msys2 msys` shell, aka `msys2_shell.cmd` in the tutorial.

## Quickstart

Assuming that you're on the **ROOT** of your users directory. `~` on Unix (or Windows if you use git bash)

Installing:

1. `git clone --bare https://github.com/TheRustifyer/dotfiles $HOME/.cfg
(replace the URL for the *SSH* variant if you need)
2. git --git-dir=$HOME/.cfg/ --work-tree=$HOME checkout

>[!NOTE]
>
> Just copy and paste the second point for checkout and directly "install" the configuration files.
> Other tutorials configure again the (TODO link to alias) and the gitignore, which will cause merge conflicts,
> and it's completely unnecesary.

## Why?

On a regular job day (from Monday to Friday), I almost end using three different machines along the day.
My main coding workstation (for personal projects), which is just an **MSi** laptop running exclusively a *Manjaro*,
my gaming and secondary code workstation, that runs on *Windows*, and my job workstation, a corporative laptop
that runs on *Windows* but I mostly use *WSL2* these days.

By having my `dotfiles` in this repo, I can quickly go to another workstation and start to work as I like,
format my system without causing me too much trouble for losing my tools and having to reinstall them...

## How?

This is a git bare repo. The technique consists in storing a Git bare repository in a "side" folder (like $HOME/.cfg or
whatever) using a specially crafted alias so that commands are run against that repository and not the usual `.git`
local folder, which would interfere with any other *Git* repositories around.

## Friendly reminder, starting from scratch

If you haven't been tracking your configurations in a Git repository before, you can start using this technique easily with these lines:

```bash
git init --bare $HOME/.cfg
alias config='git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config config --local status.showUntrackedFiles no
echo "alias config='git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.zshrc
```

>[!NOTE]
>
> My configuration assumes that the base shell for the configuration uses [**starship**](https://starship.rs/). Obviously, feel free to use
> whatever shell suits you better.

### The alias `config`

By having the **config** alias set, now I can invoke *Git* from any place, and it will now that I am interacting
with my configuration bare repository. So I can be in any place an directly add anything that I want to be track by
my configuration repository.

