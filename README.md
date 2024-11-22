# Termux Clock
[![WIP](https://img.shields.io/badge/%F0%9F%9B%A0-WIP-cyan)](#)
[![Rust CI workflow](https://img.shields.io/github/actions/workflow/status/iahmadgad/termux-clock/rust.yml?label=Rust%20CI&logo=rust)](https://github.com/iahmadgad/termux-clock/actions/workflows/rust.yml)
[![Release workflow](https://img.shields.io/github/actions/workflow/status/iahmadgad/termux-clock/release.yml?label=Release&logo=github)](https://github.com/iahmadgad/termux-clock/actions/workflows/release.yml)
[![Lines of code](https://tokei.rs/b1/github/iahmadgad/termux-clock?category=code&label=Lines%20of%20code&style=flat)](#)
[![Latest Tag](https://img.shields.io/github/v/tag/iahmadgad/termux-clock?label=Latest%20tag&sort=semver)](https://github.com/iahmadgad/termux-clock/tags)
[![Downloads](https://img.shields.io/github/downloads/iahmadgad/termux-clock/total?label=Downloads%20(GH))](https://github.com/iahmadgad/termux-clock/releases)
[![PRs: welcome](https://img.shields.io/badge/PRs-welcome-lemon)](https://github.com/iahmadgad/termux-clock/fork)

Termux tool to set alarms & timers headlessly.

# Requirements
- [Termux](https://github.com/termux/termux-app) >= `0.118.1` _(this version solves issue termux/termux-app#3990)_
- [Termux: API](https://github.com/termux/termux-api)
- Termux packages: `termux-api`, `at`, `cronie`

# Install
> [!IMPORTANT]
> both 2 methods install `termux-api` package not the Android app, which needs to be installed manually.
> Read [Installation](https://github.com/termux/termux-api?tab=readme-ov-file#installation) on `termux/termux-api` repo to know how to install the Android app.
## Pre-built Termux packages
- Navigate to [GitHub Releases](https://github.com/iahmadgad/termux-clock/releases), and choose package with your desired version & phone architecture.
- copy deb file url & download it using `wget` or `curl`.
- Install the downloaded package with `apt`:
```sh
apt install ./<package>.deb
```
## From source
- Install dependencies
```sh
pkg install termux-api at cronie
```
- Compile and install from the latest commit on `main` branch:
```sh
cargo install --force --git https://github.com/iahmadgad/termux-clock
```
- Or from tag (recommended):
```sh
cargo install --force --git https://github.com/iahmadgad/termux-clock --tag <tag>
```
See [`cargo-install(1)`](https://doc.rust-lang.org/cargo/commands/cargo-install.html) for more `cargo install` options.
# Development
## Requirements 
- [just](https://github.com/casey/just) >= `1.19.0` _(for modules feature)_
- [yq](https://github.com/mikefarah/yq)
- [cross](https://github.com/cross-rs/cross) _(in case you are not building on termux)_
- [termux-create-package](https://github.com/termux/termux-create-package)
- bash

Firstly install just:
```sh
cargo install just
```
other tools (except bash) will be installed automatically during build process, or when you use `install-tool` recipe.

`install-tool` recipe are expected to run on debian-based distros. if you are using other distros or termux, it is better to install them manually.
## Recipes
if you don't know just recipes read [just documentation](https://just.systems/man/en/).
| Recipe | Details |
| ------ | ------- |
| `build *FLAGS` | - |
| `test *FLAGS` | - |
| `install` | install as crate with cargo, installed binary will be stored in: `~/.cargo/bin/`. |
| `install-deb` | install as termux deb package with apt, installed binary will be stored in: `/data/data/com.termux/files/usr/bin/`. |
| `install-termux-deps` | install package dependencies with apt. |
| `install-tool +ARGS` | install tool/s used in build process. |
| `fetch-version` | fetch package version and print it. |
| `cross-build +ARGS` | cross build for android targets. |
| `cross-build-all` | cross build for all valid android targets. equivalent to: `cross-build --all`. |
| `deb-manifest +ARGS` | generate termux deb packages manifests. generated manifests are stored in: `deb/manifests/`. |
| `deb +ARGS` | build termux deb packages. built packages are stored in: `deb/packages/`.|
| `deb-all` | build termux deb packages for all target. equivalent to: `deb --all`. |
| `deb-native` | build termux deb package for native target _(in case you are using Termux)_. equivalent to: `deb --native`. |
| `clean` | clean cache. |

Other recipes can be found in `justfile` and `justmodules` directory.
# Usage
## `timer`
### `-H, --hours <HOURS>`
add hours to timer length.
### `-M, --minutes <MINUTES>`
add minutes to timer length.
### `-S, --seconds <SECONDS>`
add seconds to timer length.
### `-m, --message <MESSAGE>`
timer message.
### `-v, --vibrate`
enable vibration.
> [!NOTE]
> `-v, --vibrate` option is not available in android timers, so use it with `--termux` option.
### `-t, --termux`
set timer in termux instead of android alarm clock.
### Example
```sh
termux-clock timer -l 60 -t -m "Timer for 1 minute in Termux"
```
## `alarm`
### `-H, --hour <HOUR>`
alarm hour.
> [!NOTE]
> Hour is supposed to be `am` by default, to make an alarm at `pm` hour add `12` to hour value.
### `-M, --minutes <MINUTES>`
alarm extra minutes.
### `-d, --days <DAYS>`
days to recurr the alarm, denoted by comma-seperated numbers between double quotes, e. g. `1,2,3`, where each number corresponds to a weekday, starting from Sunday in android alarms, or Monday in termux alarms.
### `-m, --message <MESSAGE>`
alarm message.
### `-v, --vibrate`
enable vibration.
### `-t, --termux`
set alarm in termux instead of android alarm clock.
### Example
```sh
termux-clock alarm -H 6 -M 30 -d "1 2 3" -v -m "Alarm at 06:30 am 🕡 every Sunday, Monday and Tuesday with vibration enabled 📳"
```
