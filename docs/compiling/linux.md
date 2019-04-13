# Compiling Guide for Linux (Ubuntu 16.04)

## Clone

```
git clone https://github.com/LordSoylent/kittycoin-1
cd kittycoin-1
```

## Dependencies

(Prefix commands with "sudo" if not running as root, however, I recommend using root / superuser for easier compiling)

Base Ubuntu packages:

`apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev`

 Library     | Purpose          | Description
 ------------|------------------|----------------------
 libssl      | SSL Support      | Secure communications
 libboost    | Boost            | C++ Library
 miniupnpc   | UPnP Support     | Firewall-jumping
 libdb4.8    | Berkeley DB      | Wallet storage
 qt          | GUI              | GUI toolkit

Install all system Dependencies for Ubuntu 16.04:

```
apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev
apt-get install libboost-all-dev
apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
```


To install the database:

```
Kittycoin_ROOT=$(pwd)

# Pick some path to install BDB to, here we create a directory within the Kittycoin directory
BDB_PREFIX="${Kittycoin_ROOT}/db4"
mkdir -p $BDB_PREFIX

# Fetch the source and verify that it is not tampered with
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c
# -> db-4.8.30.NC.tar.gz: OK
tar -xzvf db-4.8.30.NC.tar.gz

# Build the library and install to our prefix
cd db-4.8.30.NC/build_unix/
#  Note: Do a static build so that it can be embedded into the exectuable, instead of having to find a .so at runtime
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make install

cd $Kittycoin_ROOT
```

To install miniupnpc:

Download: http://miniupnp.tuxfamily.org/files/download.php?file=miniupnpc-1.6.tar.gz

Build:

```
tar -xzvf miniupnpc-1.6.tar.gz
cd miniupnpc-1.6
make && make install
```

## Compile
```
cd $Kittycoin_ROOT/src
make -f makefile.unix
```

Compilation should now proceed!

## Purr
If your compile succeeded, congrats! **Purr time!**

You should be able to see `kittycoin-qt` in the current directory, if so:

```
chmod +x kittycoin-qt
./kittycoin-qt
```

Kittycoin-qt should now be running!
