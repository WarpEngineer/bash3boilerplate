[This document is formatted with GitHub-Flavored Markdown.   ]:#
[For better viewing, including hyperlinks, read it online at ]:#
[https://github.com/kvz/bash3boilerplate/blob/master/FAQ.md  ]:#

## Contents

* [What is a cli](#what-is-a-cli)?
* [How do I add a command-line flag](#how-do-i-add-a-command-line-flag)?
* [How do I access the value of a command-line argument](#how-do-i-access-the-value-of-a-command-line-argument)?
* [How do I incorporate BASH3 Boilerplate into my own project](#how-do-i-incorporate-bash3boilerplate-into-my-own-project)?
* [What is a magic variable](#what-is-a-magic-variable)?
* [How do I incorporate BASH3 Boilerplate into my own project](#how-do-i-incorporate-bash3boilerplate-into-my-own-project)?
* [How can I contribute to this project](#how-can-i-contribute-to-this-project)?

<!--more-->

# Frequently Asked Questions

## What is a cli?

A 'cli' is a [command-line interface](https://en.wikipedia.org/wiki/Command-line_interface).

## How do I incorporate BASH3 Boilerplate into my own project?

You can incorporate BASH3 Boilerplate into your project one of three ways:
1. Copy the desired portions of [main.sh](./main.sh) into your own script.
1. Download [main.sh](./main.sh) and start pressing the delete-key for unwanted things

Once the `main.sh` has been tailor-made for your project you could either append your own script in the same file, or source it:

1. Copy [main.sh](./main.sh) into the same directory as your script and then edit and embed it into your script using bash's dot (`.`) include feature, e.g.
```bash
#!/usr/bin/env bash
. main.sh
```
1. Source [main.sh](./main.sh) in your script or at the command line
```bash
#!/usr/bin/env bash
. main.sh
```

## How do I add a command-line flag?

1. Copy the line the main.sh [read block](https://github.com/kvz/bash3boilerplate/blob/master/main.sh#L53) that most resembles the desired behavior and paste the line into the same block.
1. Edit the single-character (e.g., -d) and, if present, the multi-character (e.g., --debug) versions of the flag in the copied line.  
1. Omit the "[arg]" text in the copied line if the desired flag takes no arguments.
1. Omit or edit the text after "Default:" to set or not set default values, respectively. 
1. Omit the "Required." text if the flag is optional.

## How do I access the value of a command-line argument?

To evaluate the value of an argument, append the corresponding single-character flag to the text `$arg_`.  For example, if the [read block]
contains the line
```bash
   -t --temp  [arg] Location of tempfile. Default="/tmp/bar"
```

then you can evaluate the corresponding argument and assign it to a variable as follows:

```bash
__temp_file_name="${arg_t}"
```

## What is a magic variable?

The [magic variables](https://github.com/kvz/bash3boilerplate/blob/master/main.sh#L63) in `main.sh` are special in that they have a different value, depending on your environment. You can use `${__file}` to get a reference to your current script, `${__dir}` to get a reference to the directory it lives in. This is not to be confused with the location of the calling script that might be sourcing the `${__file}`, which is accessible via `${0}`, and the current directory of the administrator running the script, accessible via `$(pwd)`. Other magic variables are for instance `${__os}` which currently is limited to telling you wether you are on `OSX` and otherwise defaults to `Linux`.

## How do I submit an issue report?

Please visit our [Issues](https://github.com/kvz/bash3boilerplate/issues) page.

## How can I contribute to this project?

Please fork this repository.  Then create a branch containing your suggested changes and submit a pull request based on the master branch
of <https://github.com/kvz/bash3boilerplate/>. We're a welcoming bunch, happy to accept your contributions!

## Why are you typing BASH in all caps?

As an acronym, Bash stands for Bourne-again shell, and is usually written with one uppercase. 
This project's name however is "BASH3 Boilerplate" as a reference to 
"[HTML5 Boilerplate](https://html5boilerplate.com/)", which was founded to serve a similar purpose, 
only for crafting webpages. 
Somewhat inconsistent but true to Unix ancestry, the abbreviation for our project is "b3bp".

## How can I locally develop and preview the b3bp website?

You should have a working Node.js >=10 and Ruby >=2 install on your workstation. Afterwards, you can run:

```bash
npm run web:preview
```

This will install and start all required services and automatically open a webbrowser that reloads as soon as you make any changes to the source.

The source mainly consists of:

- `./README.md` Front page
- `./FAQ.md` FAQ page
- `./CHANGELOG.md` Changelog page
- `./website/_layouts/default.html` Design in which all pages are rendered
- `./website/public/app.js` Main JS file
- `./website/public/style.css` Main CSS file

The rest is dark magic you should probably steer clear from : )

Any changes should be proposed as PRs. Anything added to `master` is automatically deployed using a combination of Travis CI and GitHub Pages.

## You're saying you are portable - why won't b3bp code run in dash / busybox / posh / ksh / mksh / zsh?

When we say _portable_, we mean across Bash versions. Bash is widespread and most systems
offer at least version 3 of it. Make sure you have that available, and b3bp will work for you.

We run automated tests to make sure that it will, here's proof for the following platforms:

- [Linux](https://travis-ci.org/kvz/bash3boilerplate/jobs/109804166#L91) `GNU bash, version 4.2.25(1)-release (x86_64-pc-linux-gnu)`
- [OSX](https://travis-ci.org/kvz/bash3boilerplate/jobs/109804167#L2453) `GNU bash, version 3.2.51(1)-release (x86_64-apple-darwin13)`

This portability however does not mean we try to be compatible with 
KornShell, Zsh, posh, yash, dash or other shells. We allow syntax that would explode if 
you pasted it in anything but Bash 3 and up.

## How do I access a potentially unset (environment) variable?

The set -o nounset line in main.sh causes error termination when an unset environment variables is detected as unbound. There are multiple ways to avoid this.

Some code to illustrate:

```bash
# method 1
echo ${NAME1:-Damian} # echos Damian, $NAME1 is still unset
# method 2
echo ${NAME2:=Damian} # echos Damian, $NAME2 is set to Damian
# method 3
NAME3=${NAME3:-Damian}; echo ${NAME3} # echos Damian, $NAME3 is set to Damian
```

This subject is briefly touched on as well in the Safety and Portability section under point 5. b3bp currently uses [method 1](https://github.com/kvz/bash3boilerplate/blob/master/main.sh#L85) when we want to access a variable that could be undeclared, and [method 3](https://github.com/kvz/bash3boilerplate/blob/master/main.sh#L46) when we also want to set a default to an undeclared variable, because we feel it is more readable than method 2. We feel `:=` is easily overlooked, and not very beginner friendly. Method 3 seems more explicit in that regard in our humble opinion.
