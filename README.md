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
  echo "I am ${FUNCNAME[0]}ing"
}

deploy() {
  # doit deploy a b c
  echo "I am ${FUNCNAME[0]}ing with args '$1 $2 $3'" # I am deploying with $1=a $2=b and $3=c
}

clean() { echo "I am ${FUNCNAME[0]}ing in just one line."; }

_preflight() {
  echo "I am a hidden task because I start with _. You can still call me directly"
  which docker || (echo "Error: Docker is not installed"; exit 1) # Check for docker.
}

all() {
  _preflight && clean && build && deploy
}

"$@" # <- Do it.

[ "$#" -gt 0 ] || build # Default
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

## Related Tools

* [Task](http://taskfile.org/#/usage) a task runner / simpler Make alternative written in Go
* [Robo](https://github.com/tj/robo) Simple Go / YAML-based task runner for the team.
* [godo](https://github.com/go-godo/godo) godo is a task runner and file watcher for golang in the spirit of rake, gulp. It has kind of same name.
* [just](https://github.com/casey/just) A better make in rust.
