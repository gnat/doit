# Build without bullshit.
![party-palpatine](https://user-images.githubusercontent.com/24665/174114761-42dfba9c-dcae-473b-8d83-aee59629f7aa.gif)

## 🏴‍☠️ Anti-features
* No installation
* No dependencies
* No overhead

Replace your convoluted build system with vanilla bash.

## Show me

```bash
#!/bin/bash
set -euo pipefail # Error handling: -e stops on errors. -u stops on unset variables. -o pipefail stops pipelines on fail: https://mobile.twitter.com/b0rk/status/1314345978963648524

build() {
  echo "I am ${FUNCNAME[0]}ing" # doit build ... I am building
}

deploy() {
  echo "I am ${FUNCNAME[0]}ing with args $1 $2 $3" # doit deploy a b c ... I am deploying with args a b c
}

clean() { echo "I am ${FUNCNAME[0]}ing in just one line."; }

required() {
  which docker || { echo "Error: Docker is not installed"; exit 1; }
}

all() {
  required && clean && build # Continues chain on success.
}

# Optionally, run any script from your own URL.
remote() {
  echo "Not found: '$1' Trying remote..."
  { curl -fsSL https://raw.githubusercontent.com/gnat/doit/main/ops/$1.sh ${@:2} | bash; } || echo "Not found: '$1'"
  # Add your own public or private repositories!
}

"$@" || remote $@ # DO IT.
[ "$#" -gt 0 ] || echo "Usage: $0 task [args]" # Or, default.
```
Save as `doit.sh` use `chmod +x ./doit.sh`

Do it: `./doit.sh`
Or, do a task: `./doit.sh build`

## Alias setup
* `echo "alias doit='./doit.sh'" >> ~/.bashrc`
* Open new shell.
* You can now use `doit`

## Nice snippets

### Generate help message
```bash
[ "$#" -gt 0 ] || printf "Usage:\n\t$0 ($(compgen -A function | grep '^[^_]' | paste -sd '|' -))\n"
```
* Hide task from help message by starting with "_". You can still call them directly.

### Helper library
```bash
source $(dirname $0)/helpers.sh # Include file.
```

### Timestamps
```bash
tsHHMMSS() { echo "$(( ${1} / 3600 ))h $(( (${1} / 60) % 60 ))m $(( ${1} % 60 ))s"; }
```

## Helpful

* [Bash Cheat Sheet](https://bertvv.github.io/cheat-sheets/Bash.html)
* [Bash Variable Parameter Expansions](https://www.cyberciti.biz/tips/bash-shell-parameter-substitution-2.html)
* [Why "pipefail"?](https://mobile.twitter.com/b0rk/status/1314345978963648524)


## Original concept and special thanks
* Forked from [do](https://github.com/8gears/do) and [run](https://github.com/icetbr/run)
* Inspired by [shell](https://github.com/netkiller/shell)

## Related Tools

* [Task](http://taskfile.org/#/usage) a task runner / simpler Make alternative written in Go
* [Robo](https://github.com/tj/robo) Simple Go / YAML-based task runner for the team.
* [godo](https://github.com/go-godo/godo) godo is a task runner and file watcher for golang in the spirit of rake, gulp. It has kind of same name.
* [just](https://github.com/casey/just) A better make in rust.
