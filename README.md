# 🧱 Tor Static for Windows

A fully **statically linked Tor build** for Windows — no external DLLs, no dependencies, no setup hassle.  
Built with **MSYS2 + MinGW64**, this binary runs completely standalone on Windows.

> ⚠️ Note: This project is **not affiliated with or endorsed by the Tor Project**.  
> It’s a community-built version meant for educational and convenience purposes only.  
> Always verify any binary you download.

---

## 🧩 Why This Exists

If you’ve ever tried compiling Tor on Windows… you know.  
Static linking is *possible* — but painful. Dependency hell, path quirks, library conflicts, and random build errors that make you question life choices.  

So, I decided to go through that pain once, get it working cleanly, and share both:
- A **ready-to-use static build** (`tor.exe`)
- A **step-by-step guide** on how to build it yourself

---

## ⚙️ Build Environment

This static build was produced using:

- **Windows 10/11 (x64)**
- **MSYS2 (MINGW64 shell)**
- **gcc (MinGW-w64)**  
- **OpenSSL**, **libevent**, and **zlib** — statically linked  
- No shared `.dll` dependencies

You can verify that by running:
```bash
ldd tor.exe
````

If you see “not a dynamic executable”, it’s fully static.

---

## 🧰 How to Build It Yourself

If you want to recreate it (or just trust your own binary), here’s how I did it.

### 1. Install MSYS2

Grab it from [https://www.msys2.org](https://www.msys2.org)
Open **MSYS2 MINGW64** terminal after installing.

### 2. Install dependencies

```bash
pacman -Syu
pacman -S git make autoconf automake libtool mingw-w64-x86_64-gcc \
    mingw-w64-x86_64-openssl mingw-w64-x86_64-zlib \
    mingw-w64-x86_64-libevent pkg-config
```

### 3. Clone and configure Tor

```bash
git clone https://git.torproject.org/tor.git
cd tor
./autogen.sh
./configure --enable-static-tor --disable-asciidoc \
    --disable-system-torrc \
    --with-libevent-dir=/mingw64 \
    --with-openssl-dir=/mingw64 \
    --with-zlib-dir=/mingw64 \
    LDFLAGS="-static"
```

### 4. Build it

```bash
make -j$(nproc)
```

If you get a `/tmp/tor_lib_combining` error, rebuild single-threaded:

```bash
make clean && make -j1
```

### 5. Find your build

Output will be at:

```
src/app/tor.exe
```

To copy it to your desktop:

```bash
cp src/app/tor.exe /c/Users/$(whoami)/Desktop/
```

---

## 🧪 Verify the Build

Check version:

```bash
tor.exe --version
```

Check static linking:

```bash
ldd tor.exe
```

It should respond:

```
not a dynamic executable
```

You can also upload it to [VirusTotal](https://www.virustotal.com/) — static Tor binaries often trigger **false positives** (because of network behavior and static linking), but they’re harmless if built from source.

---

## 🧾 License & Credits

* Tor is licensed under **BSD-3-Clause** — original source: [https://git.torproject.org/tor.git](https://git.torproject.org/tor.git)
* This repository’s scripts and instructions are under **GPL-3.0**
* Built and maintained by [YourNameHere]

---

## 💬 Notes

This repo includes:

* `/build/tor.exe` — prebuilt static binary
* `/docs/BUILD.md` — this guide
* `/scripts/` — build scripts used in the process

If you want to rebuild, you can simply rerun the commands or use the `build_static_tor.sh` script.

---

## 🚀 Quick Summary

| Step         | Command                                        | Description             |
| ------------ | ---------------------------------------------- | ----------------------- |
| Install deps | `pacman -S ...`                                | Get everything you need |
| Clone source | `git clone https://git.torproject.org/tor.git` | Get Tor code            |
| Configure    | `./configure ... LDFLAGS="-static"`            | Enable static build     |
| Build        | `make -j$(nproc)`                              | Compile it              |
| Verify       | `ldd tor.exe`                                  | Confirm static linking  |

---

## ❤️ Why Share It

Because nobody deserves to lose a weekend fighting MinGW just to build one static binary.
If this saves someone a few hours (or gray hairs), it was worth it.

---

> **Disclaimer:**  
> This project has been vibe-coded please use it cautiously. - Masterman331

---

## 🪙 Support This Project

If you found this project useful, interesting, or helpful, consider supporting its development through **Monero**.

<p align="center">
  <img src="https://raw.githubusercontent.com/masterman331/masterman331/main/moneroadress.png" alt="Monero donation QR code" width="220"/>
</p>

<p align="center">
<code>47chh1Z9wvHDP6ZDpzPPETKaXUfsNnmXr8P5cL4ofAkH1fi3mrrvC7tiRoeqxtNCbB1BQ3rqk5k2tSPGoiMSTUTC3iPc9Qu</code>
</p>

<p align="center">
  <em>Privacy-respecting contributions help keep independent development alive.</em>
</p>
