# Internet Computer (IC) Development — Full Setup Guide

**Platform:** Windows 10+ (WSL Ubuntu)
**Backend:** Motoko
**Frontend:** React
**Node Version:** 20.x (latest LTS compatible with dfx)
**DFX Version:** Latest stable (e.g., 0.30.x)

---

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


---


# `hello`

Welcome to your new `hello` project and to the Internet Computer development community. By default, creating a new project adds this README and some template files to your project directory. You can edit these template files to customize your project and to include your own code to speed up the development cycle.

To get started, you might want to explore the project directory structure and the default configuration file. Working with this project in your development environment will not affect any production deployment or identity tokens.

To learn more before you start working with `hello`, see the following documentation available online:

- [Quick Start](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-locally)
- [SDK Developer Tools](https://internetcomputer.org/docs/current/developer-docs/setup/install)
- [Motoko Programming Language Guide](https://internetcomputer.org/docs/current/motoko/main/motoko)
- [Motoko Language Quick Reference](https://internetcomputer.org/docs/current/motoko/main/language-manual)

If you want to start working on your project right away, you might want to try the following commands:

```bash
cd hello/
dfx help
dfx canister --help
```

## Running the project locally

If you want to test your project locally, you can use the following commands:

```bash
# Starts the replica, running in the background
dfx start --background

# Deploys your canisters to the replica and generates your candid interface
dfx deploy
```

Once the job completes, your application will be available at `http://localhost:4943?canisterId={asset_canister_id}`.

If you have made changes to your backend canister, you can generate a new candid interface with

```bash
npm run generate
```

at any time. This is recommended before starting the frontend development server, and will be run automatically any time you run `dfx deploy`.

If you are making frontend changes, you can start a development server with

```bash
npm start
```

Which will start a server at `http://localhost:8080`, proxying API requests to the replica at port 4943.

### Note on frontend environment variables

If you are hosting frontend code somewhere without using DFX, you may need to make one of the following adjustments to ensure your project does not fetch the root key in production:

- set`DFX_NETWORK` to `ic` if you are using Webpack
- use your own preferred method to replace `process.env.DFX_NETWORK` in the autogenerated declarations
  - Setting `canisters -> {asset_canister_id} -> declarations -> env_override to a string` in `dfx.json` will replace `process.env.DFX_NETWORK` with the string in the autogenerated declarations
- Write your own `createActor` constructor
