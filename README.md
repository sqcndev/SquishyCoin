# SquishyOcean (Squishy-qt)

![](./doc/images/Squishy-logo2.png)

Squishy-Qt (SquishyOcean) is a Qt native wallet for SQCN. It's available for three OS platforms - Windows, Linux, MacOS.

Use the following scripts to build:

- Linux: `build.sh` (native build)
- Windows: `build-win.sh` (cross-compilation for Win)
- MacOS: `build-mac-cross.sh` (cross-compilation for OSX)
- MacOS: `build-mac.sh` (native build)

## Coin Info 
- 23,000,000 max coin amount
- 100 coin block reward for era 1 (6 total eras 100, 25, 12.5, 6.25, 3.125, 1.5625)
- 7 min block time
- 25% staking rewards
- 1,468,750 pre-mine (6.4% of total coin) to be used for exchange listing, liquidity, faucet.
	Community to vote on other uses, wallet address to be posted publicly!
## Connect

Discord Server ([SquishyCoin](https://discord.gg/zxbBrzAqhZ))

## How to build? ##

#### Linux

```shell
#The following packages are needed:
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python3 bison zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl
```

```shell
git clone https://github.com/sqcndev/SquishyCoin.git
cd Squishycoin-core-wallets
./zcutil/fetch-params.sh
# -j8 = using 8 threads for the compilation - replace 8 with number of threads you want to use
./zcutil/build.sh -j8
#This can take some time.
```

#### OSX (Cross-compile)

Before start, read the following docs: [depends](https://github.com/bitcoin/bitcoin/blob/master/depends/README.md), [macdeploy](https://github.com/bitcoin/bitcoin/blob/master/contrib/macdeploy/README.md) .

Install dependencies:
```
sudo apt-get install curl librsvg2-bin libtiff-tools bsdmainutils cmake imagemagick libcap-dev libz-dev libbz2-dev python3-setuptools libtinfo5 xorriso
```

Place prepared SDK file `Xcode-12.1-12A7403-extracted-SDK-with-libcxx-headers.tar.gz` in repo root, use `build-mac-cross.sh` script to build.

#### OSX (Native) - Tested only on Mavericks
Ensure you have [brew](https://brew.sh) and Command Line Tools installed.
```shell
# Install brew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Install Xcode, opens a pop-up window to install CLT without installing the entire Xcode package
xcode-select --install 
# Update brew and install dependencies
brew update
brew upgrade
brew tap discoteq/discoteq; brew install flock
brew install autoconf autogen automake
# brew install gcc@6
brew install binutils
brew install protobuf
brew install coreutils
brew install wget
# Clone the Squishy repo
git clone https://github.com/sqcndev/SquishyCoin.git
# Change master branch to other branch you wish to compile
cd Squishycoin-core-wallets
./zcutil/fetch-params.sh
# -j8 = using 8 threads for the compilation - replace 8 with number of threads you want to use
./zcutil/build-mac.sh -j8
# This can take some time.
```

#### Windows
Use a debian cross-compilation setup with mingw for windows and run:
```shell
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python3 python3-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl cmake mingw-w64
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
rustup target add x86_64-pc-windows-gnu

sudo update-alternatives --config x86_64-w64-mingw32-gcc
# (configure to use POSIX variant)
sudo update-alternatives --config x86_64-w64-mingw32-g++
# (configure to use POSIX variant)

sudo bash -c "echo 0 > /proc/sys/fs/binfmt_misc/status"
#temp build patch

git clone https://github.com/sqcndev/SquishyCoin.git
cd Squishycoin-core-wallets
./zcutil/fetch-params.sh
# -j8 = using 8 threads for the compilation - replace 8 with number of threads you want to use
./zcutil/build-win.sh -j8
#This can take some time.
```
**squishy is experimental and a work-in-progress.** Use at your own risk.

*p.s.* Currently only `x86_64` arch supported for MacOS, build for `Apple M1` processors unfortunately not yet supported.

## Create SQCN.conf ##

Before start the wallet you should [create config file](https://github.com/sqcndev/SquishyCoin.git/wiki/F.A.Q.#q-after-i-start-squishy-qt-i-receive-the-following-error-error-cannot-parse-configuration-file-missing-squishyconf-only-use-keyvalue-syntax-what-should-i-do) `SQCN.conf` at one of the following locations:

- Linux - `~/.squishy/SQCN.conf`
- Windows - `%APPDATA%\Squishy\SQCN.conf`
- MacOS - `~/Library/Application Support/Squishy/SQCN.conf`

With the following content:

```
txindex=1
rpcuser=squishy
rpcpassword=local321 # don't forget to change password
rpcallowip=127.0.0.1
rpcbind=127.0.0.1
server=1
rpcport=xxxxx
txindex=1
rpcworkqueue=256
addnode=146.190.69.236
addnode=143.198.59.2
```

Bash one-liner for Linux to create `SQCN.conf` with random RPC password:

```
RANDPASS=$(tr -cd '[:alnum:]' < /dev/urandom | fold -w16 | head -n1) && \
tee -a ~/.squishy/SQCN.conf << END
txindex=1
rpcuser=squishy
rpcpassword=${RANDPASS}
rpcallowip=127.0.0.1
rpcbind=127.0.0.1
server=1
rpcport=xxxxx
txindex=1
rpcworkqueue=256
addnode=146.190.69.236
addnode=143.198.59.2

END
```

## Developers of Qt wallet ##

- Main developer: **Ocean**
- IT Expert / Sysengineer: **Decker**
- Squishy Coin developers
