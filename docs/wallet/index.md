---
sidebar_label: Nym Desktop Wallet
description: "Setup and use the Nym Tauri Desktop Wallet."
hide_title: false
title: Nym Desktop Wallet
---

A Rust and Tauri desktop wallet implementation which gives access to the Nym mixnet, serving as a point of connection to interact with your node and to delegate stake to others! 

:::note 

Unlike the rest of the Nym codebase, the Nym Desktop Wallet is currently built via Yarn. Follow this doc for the instructions to do so.

:::

### Building the Nym Desktop Wallet

#### Prerequisites

- `git`

```
sudo apt update && sudo apt install git
```

Verify `git` is installed with:

```
git version
# Should return: git version X.Y.Z
```

- `Yarn` 

- `NodeJS >= v16.8.0 <= v17.0.1`

Check your version with: 

```
node -v
# Should return: v16.8.0
```

It is recommended to use [nvm](https://github.com/nvm-sh/nvm) for NodeJS versions management. 

- `Rust & cargo >= v1.56`

Verify the version and if cargo is in your path:

```
cargo --version
# Should return: cargo 1.56.0 (4ed5d137b 2021-10-04)
```

If cargo is not in your $PATH, then add it with following command(depends on your installation and user) to your .bashrc or other:

```
echo "export PATH=$PATH:$HOME/cargo/env" | tee -a ~/.bashrc
```

We recommend using the [Rust shell script installer](https://www.rust-lang.org/tools/install). Installing cargo from your package manager (e.g. `apt`) is not recommended as the packaged versions are usually too old.

If you really don't want to use the shell script installer, the [Rust installation docs](https://forge.rust-lang.org/infra/other-installation-methods.html) contain instructions for many platforms.

In case your Rust & cargo version are older, simply use `rustup update` to get the latest build. 

#### Additional prerequisites for Linux (Debian-based distros)
```
sudo apt update && sudo apt install libwebkit2gtk-4.0-dev \
    build-essential \
    pkg-config \
    libssl-dev \
    libgtk-3-dev \
    libappindicator3-dev \
    patchelf \
    librsvg2-dev
```
Without some of these you might run into issues related to gtk 

#### Additional prerequisites for Windows

- When running on Windows you will need to install c++ build tools
- An easy guide to get rust up and running [Installation]("http://kennykerr.ca/2019/11/18/rust-getting-started/")
- When installing NodeJS please use the `current features` version
- Using a package manager like [Chocolatey]("chocolatey.org") is recommended

#### Installation

Inside of the `tauri-wallet` folder, run the following commands

```
yarn install
```

### Running in Development Mode

You can run the wallet without having to install it in development mode by running the following terminal command from the `tauri-wallet` folder

```
yarn dev
```

This will then start the Wallet GUI. 

### Running in Production Mode

To build and install the wallet, run the following terminal command from the `tauri-wallet` folder

```
yarn build
```

This will build an executable file that you can use to install the wallet on your machine. 

### Install the wallet

Once the the building process is complete an installation file can be found in: `tauri-wallet/target/release/nym_wallet`. 
