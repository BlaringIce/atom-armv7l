# atom-armv7l
:wave: Hello! This repository contains several patched files and instructions that allow you to build and run Atom 1.24.0 on `armv7l` machines like Raspberry Pi's.

![Screenshot](https://raw.githubusercontent.com/hypersad/atom-armv7l/master/assets/screenshot.png)
## Building
### Gettings sources & building
Firstly, install basic prerequisites as it described in [Atom Flight Manual](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#building).

Clone Atom sources and this repository, then merge them:
```
git clone https://github.com/atom/atom.git
cd atom
git checkout v1.24.0
cd ..
git clone https://github.com/hypersad/atom-armv7l.git
cp -r atom-armv7l/script/* atom/script/
rm -rf atom-armv7l
```
Now you could start building:
```
cd atom
script/build
```
Building Atom from scratch on `armv7l` machines usually takes around 1-2 hours, but build time depends on your hardware.
### Generating startup blob separately
**These steps must be done on `i386`/`amd64` machine.**

We will use mksnapshot binaries for `i386`/`amd64` with `armv7l` as target to generate suitable startup blob.

Get suitable `mksnapshot` binary. For now it is `mksnapshot-v1.6.0-linux-armv7l`:
```
wget https://github.com/electron/electron/releases/download/v1.6.0/mksnapshot-v1.6.0-linux-armv7l.zip
unzip -j mksnapshot-v1.6.0-linux-armv7l.zip mksnapshot
rm mksnapshot-v1.6.0-linux-armv7l.zip
```
Get generated startup snapshot (`out/startup.js`) from your `armv7l` machine. You could do it via `scp`, for example:
```
scp helix@192.168.1.30:/home/helix/atom/out/startup.js startup.js
```
Generate startup blob from `startup.js` via `mksnapshot`:
```
./mksnapshot startup.js --startup_blob snapshot_blob.bin
```
Send generated startup blob back to your `armv7l` machine. You could do it via `scp` too, for example:
```
scp snapshot_blob.bin helix@192.168.1.30:/home/helix/atom/out/atom-1.24.0-armv7l/snapshot_blob.bin
```

Now, you could try to launch Atom! :tada:
