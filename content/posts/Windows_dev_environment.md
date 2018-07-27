---
title: Setting up a Web App Dev Environment under Windows
author: Marlan
date: 2018-07-28
categories:
  - Python
tags:
  - React
  - Python
  - Windows
  - Docker
output:
  blogdown::html_page:
    toc: true
---

Writing code is hard enough. But it sucks when your desktop OS seems to be actively fighting against you actually getting anything done. The best case scenario is that you're running a Unix/Linux based OS. If you're running something fairly mainstream like Ubuntu, chances are most everything you need works out the box (apart from wireless and graphics card drivers of course) and most of the documentation, tutorials and resources on the internet assume that.

More likely, if you're doing anything other than code in your normal working day then you'll probably need to be using one of the Microsoft Office Products and that means you're on Windows. Before you ask,

1.  Yes, I know Office alternatives exist but none of them are properly compatible and there came a point where I could no longer bear the broken formatting or the constant crashing of running under wine.

2. Yes, Macs are awesome and you get the best of both worlds and the design and the hardwarde and the intuitive interface and... I bloody hate Macs. There I said it. You can go throw up in the corner now.

Besides most normal people in a normal working environment are running Windows on their normal Dell or HP laptops and wouldn't be allowed to change operating systems even if they wanted to. That's a fact. I'm writing this for all you normal people out there. Some people even like windows and if you embrace the Microsoft stack then your life may be as happy as those Apple people (I wouldn't go so far as to call that normal but things have certainly gotten better recently). Thankfully, we've got a pretty good blueprint now for setting up a solid Windows environment for Web Development using Open Source tools.

These will likely require a few admin installs so schedule some time with somebody with an admin password to set this all up. There are a few windows command line tools like [scoop](https://scoop.sh/) and [chocolatey](https://chocolatey.org/) that might make this easier and not even require admin rights. One day I'll spend some time figuring out how those work and update this post accordingly - maybe I'll even add in some images. Other options include running a [vagrant](https://www.vagrantup.com/) box or [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and setting that up as per the linux instructions below.

This post will focus on living completely under Windows but using tools that are also available under linux, with minimal change required to switch over to a linux desktop machine or when deploying to a remote linux server. We'll set up a Python Flask API server with a Postgresql database on the backend and React.js frontend. Finally we'll wrap it all in some nice docker containers, so we can deploy wherever the heck we feel like it when we're done.

## Install a Console Emulator (Cmder)

You'll be spending a decent amount of time running commands from the console so you might as well make the experience enjoyable. The regular DOS-like `cmd` command prompt under windows lets you navigate through folders and run some basic commands but doesn't support most of the command line tools that you'll need. Powershell seems to be incredibly powerful these days but the syntax is completely different to linux, and even to the old DOS.

Download and install [Cmder](http://cmder.net/) console emulator. Make sure you get the full version as this will include git-for-windows and allow you to use those sweet, sweet linux commands like `ls`, `less` and `grep`.

Cmder is really customisable so spend some time customising the appearance (I don't know how people use terminals without [Quake style dropdown](https://conemu.github.io/en/SettingsQuake.html)), keyboard shortcuts and shell options. Also [place a link](https://github.com/cmderdev/cmder/issues/532) to the Cmder executable in your windows startup folder to make sure it's always there when you turn your PC on.

## Set up Version Control (Git)

If you're using anything other than [git](https://git-scm.com/) for version control... why? The Cmder installer will also take you through the git for windows installation process. Make sure you select the setting that checks out windows style line endings and pushes back linux style line endings to the repository. If you forget it later use

    git config --global core.autocrlf true

If you use a managed remote respository, you'll also want to set it up to use ssh instead of http. The major benefit of this is that you won't need to keep entering user names and passwords for git. It also helps when deploying the app on remote servers as the repository url won't be linked to any particular user's name. Follow these instructions for [Bitbucket](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html) or [Github](https://help.github.com/articles/connecting-to-github-with-ssh/).

## Install a Text Editor / IDE (Atom)

There are many good commercial IDEs out there. Most of the time though I just want a text editor that looks good, does what I want and otherwise gets out of the way. [Atom](https://atom.io/), developed by the people at Github, is free, good, has a great library of package add-ins and is infinitely hackable if you're so inclined to customise every last detail.

Personally, I found that a few good installed packages go a long way. I'd recommend:

- **file-icons** for icons that show the file type in the file tree
- **Sublime-Style-Column-Selection** block selection that is really useful when used with multiple cursors
- **auto-detect-indentation** for swapping between 2 and 4 spaced tabs
- **git-hide** hides all files that would be hidden by .gitignore files
- **linter** for linting with plugins for Python and Javascript
- Python specific
    - **Hydrogen** allows you to run .py files like .Rmd files
    - **autocomplete-python** adds autocomplete functionality
- React/Javascript specific
    - **language-babel** syntax highlighting for ES6 and JSX
    - **autocomplete-modules** for autocompleting import statements
    - **emmet** generates html tags using css syntax

Special mention goes to [VSCode](https://code.visualstudio.com/). It's open source, from Microsoft and people seem to like it. Quite frankly the only reason I haven't tried it is that I'm afraid that I might like it too and then I'd have to leave my carefully crafted Atom home.

## Install PostgreSQL (EnterpriseDB)

While there are a bunch of different open source database options to choose from including MySQL and SQLite, PostgreSQL has some nice features including PostGIS for mapping, PGCrypto for encryption and support for JSON data. The recommended installation method under Windows seems to be the installer from [EnterpriseDB](https://www.postgresql.org/download/windows/).

Once installed you can use the bundled pgAdmin tool to manage users and databases, and StackBuilder to manage packages. There's also a graphical installer from [BigSQL](https://www.openscg.com/bigsql/postgresql/installers.jsp/) which seems to be comparable.


## Install a Database Client (DBeaver)

[DBeaver](https://dbeaver.io/) is an excellent database client. It connects to most relational databases that support JDBC connections including MySQL, PostgreSQL, Vertica and SQL Server. The Community edition is free and open source and has all the GUI SQL scripting functionality you might need.

## Install Python (Miniconda)

The easiest way to get Python working under Windows is via the [Anaconda](https://www.anaconda.com/download) distribution. It comes bundled with a suite of scientific computing applications including Jupyter, RStudio as well the Spyder and Rodeo scientific IDEs.

These are great and all, but actually we just want Python and **conda**, the package and environment manager. For that we turn to [Miniconda](https://conda.io/miniconda.html). During the installation, remember to tick the box that adds the python executable to your PATH variable.

Once installed, create new virtual environments with

    conda create --name env_name python=3.6

Switch to the environment with

    activate env_name

Exit the environment with

    deactivate

Although conda's main purpose is package management, I still prefer using regular `pip install`. There's no guarantee somebody else using your code will be using conda as well, so if you can manage to get all your packages installed with pip then anybody else should as well.

## Install Create React App (Node.js and Yarn)

[React](https://reactjs.org/) fundamentally changed the way I thought about front end development. To be honest, I found my first introduction to web page applications really confusing. I wasn't quite sure when a given piece of code would run, and all those $ signs in JQuery freaked me the hell out.

The composability of React components writing HTML directly within the Javascript using JSX and the logical flow of lifecycle methods made sense to me. Since you had to have a build system to transpile all the code, requiring other node modules to include in your code was easy.

That build system thing was a bit of a bugger though. Although writing code was a joy, I had no idea how to set up a build system from scratch for a new project. Gulp, Grunt and Bower seemed way more complicated than they needed to be for me to get a simple Hello World going.

Then along came [Create React App](https://github.com/facebook/create-react-app) which generated all the boilerplate you need to start a new React app with opinionated but sensible defaults for a well configured [Webpack](https://webpack.js.org/) build system including Webpack development server that automatically handled hot reloading.

To get all this going you need to install [Node](https://nodejs.org/en/) and a package manager. The default package manager [npm](https://www.npmjs.com/) occasionally fails for reasons I can't explain so I prefer using [Yarn](https://yarnpkg.com/en/docs/install#windows-stable). Thankfully both Node and Yarn install quite happily under Windows, so just grab the installers from the respective sites. When you need to upgrade them, download the new version of the installers and simply run them again.

To start a new React project run

    yarn create react-app

Install packages for an existing app using

    yarn

And launch the development server using

    yarn start

## Install React and Redux Devtools

If you're using React, and your app gets complex enough, you probably want to use [Redux](https://redux.js.org/) for state management. Both [React](https://github.com/facebook/react-devtools) and [Redux](https://github.com/zalmoxisus/redux-devtools-extension) have great debugging tools built into the Browser Devtools, available for both Chrome and Firefox.

Your Redux store will need a bit of modification to work with Redux Devtools. Follow the instructions on the Github page and all should be well.

## Install Docker (Docker Toolbox)

Officially Docker has first class Windows support through [Docker for Windows](https://www.docker.com/docker-windows). A bit of a gotcha though is that it requires Windows 10 **Professional** so if you've got the Home or Single Language Versions, you're out of luck.

But wait, you can still get it to work through [Docker Toolbox](https://docs.docker.com/toolbox/overview/). Technically this just launches a very minimal virtual machine in VirtualBox and then installs a MinGW console to connect to it. So we're straying slightly from our "living completely under Windows" objective but it's the last little piece and we're tired.

One thing we can do once we've got Docker Toolbox installed is run that blasted little MinGW Docker console in Cmder so at least it's consistent with everything else. Follow the instructions on [Jay Vilalta's Blog](https://jayvilalta.com/blog/2016/05/02/running-docker-toolbox-inside-conemu/) to get that working.

A key difference between this setup and having Docker run natively is that the docker containers will not be accessible on `localhost` but rather on a different IP, generally `192.168.99.100`. This is pretty much the same as what you'd have to deal with when you deployed onto a remote server so in a way it's possibly better this way.

## Conclusion

That was quite a mission but now we have a pretty functional Windows dev environment suitable for our Flask-React Web App stack.

In Part II (coming soon) we'll actually build such an app, deploy it and discuss the differences between Development and Production environments.
