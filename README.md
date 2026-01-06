# Internet Computer (IC) Development — Full Setup Guide
Learn how to set up Internet Computer (DFINITY) development on Windows with WSL Ubuntu, Node 20, DFX, Motoko, and React.

**Platform:** Windows 10+ (WSL Ubuntu)
**Backend:** Motoko
**Frontend:** React
**Node Version:** 20.x (latest LTS compatible with dfx)
**DFX Version:** Latest stable (e.g., 0.30.x)

---

### Quick start with the default `hello` project

If you just want to **clone and run the `hello` project locally** on your WSL Ubuntu without going through the full IC setup, see the automatically generated README included in this project:
- [`HELLO_README.md`](./HELLO_README.md) 

This explains how to clone the repo and start the DFX canisters and frontend locally.

---

### Full IC + WSL Setup (Recommended for first-time users)

If you want the **complete step-by-step setup** including WSL Ubuntu, Node 20, DFX installation, VSCode + Motoko extension, and automatic creation of the `hello` project, follow the guide below

## 1️⃣ Install WSL + Ubuntu

1. Open **Windows PowerShell** as Administrator.
2. Install WSL:

```powershell
wsl --install
```

3. Restart your PC.
4. Set Ubuntu username & password.
5. Verify installation:

```powershell
wsl --list --verbose
```

---

## 2️⃣ Install VSCode + Extensions

1. Download and install [VSCode](https://code.visualstudio.com/).
2. Install extensions inside VSCode:

   * **Motoko language support** (Dfinity)
   * **Remote - WSL**

---

## 3️⃣ Install Homebrew in WSL

Open Ubuntu and write this:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Add Homebrew to PATH:

```bash
echo >> ~/.bashrc
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

Install build tools:

```bash
sudo apt-get update
sudo apt-get install build-essential -y
```

Verify Homebrew:

```bash
brew --version
```

---

## 4️⃣ Install Node.js (v20.x LTS)

```bash
brew install node@20
brew link node@20 --force
```

Verify Node & npm:

```bash
node -v   # v20.x
npm -v    # >=10.x
```

---

## 5️⃣ Install DFX (latest stable)

```bash
sh -ci "$(curl -fsSL https://sdk.dfinity.org/install.sh)"
```

Add DFX to PATH:

```bash
source "$HOME/.local/share/dfx/env"
```

Verify:

```bash
dfx --version   # should show latest stable, e.g., 0.30.1
```

---

## 6️⃣ Create IC Projects Folder

```bash
mkdir ~/ic-projects
cd ~/ic-projects
```

---

## 7️⃣ Create New Hello DApp

```bash
dfx new hello
cd hello
```

* Backend: `Motoko`
* Frontend: `React`
* Extra features: optional (Internet Identity, Bitcoin Regtest, tests)

Check project structure:

```bash
ls -R
```

---

## 8️⃣ Start Local IC Replica

### ⚡ Open VSCode & Connect to WSL

1. Open VSCode → `Ctrl+Shift+P` → **Remote-WSL: New Window** (connect to Ubuntu).  
2. Install **Motoko language support** extension in WSL.  
3. Open your `~/ic-projects/hello` folder in this WSL window.  


Open **terminal 1** in VSCode:

```bash
cd ~/ic-projects/hello
dfx start
```

* Wait for:

```
Replica API running on 127.0.0.1:4943
```

* Keep terminal open.

---

## 9️⃣ Deploy Canisters

Open **terminal 2** in VSCode:

```bash
cd ~/ic-projects/hello
dfx deploy
```

* Note the frontend URL (e.g., [http://127.0.0.1:8000/?canisterId=](http://127.0.0.1:8000/?canisterId=)...)

---

## 🔟 Start Frontend

In the same folder or terminal 3:

```bash
npm install
npm start
```

Open browser at `http://localhost:8080/` to see your Hello DApp running.

---

## ✅ Notes / Tips

* Always run `dfx start` in one terminal, `dfx deploy` in another.
* Node 20 + latest DFX is stable .
* If webpack / cli errors happen, check Node version: `node -v`.


