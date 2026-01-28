# Obsidian Setup Guide

  

This guide will help you set up the **DTU Salsa Data** vault on your machine, enabling you to synchronize moves and concepts.

  

## 1. Install Obsidian

  

Download and install Obsidian for your operating system:

[https://obsidian.md/download](https://obsidian.md/download)

  

## 2. Install Git

  

> [!IMPORTANT]
> Even though we use an Obsidian plugin, **Git must be installed on your system** for it to work correctly.

  

### Windows

  

1. **Download & Install**: Get Git from [git-scm.com](https://git-scm.com/download/win). Use all default settings.

2. **Verify Credential Manager**: Open a terminal (PowerShell or Command Prompt) and run:

```bash

git config credential.helper

```

Output should be `manager`. If not, run:

```bash

git config --global credential.helper manager

```

  

### macOS

  

Git is usually installed (via Xcode command line tools). If not, you can install it or check if it's available by running `git --version` in terminal.

  

**Authentication Setup**:

Run the following to use the macOS KeyChain for credentials:

  

```bash

git config --global credential.helper osxkeychain

```

  

### Linux

  

Install Git via your package manager (e.g., `sudo apt install git`).

  

**Authentication Setup**:

To avoid re-entering passwords, use `libsecret` (GNOME Keyring/KDE Wallet):

  

1. Install `libsecret` (Ubuntu/Debian):

```bash

sudo apt install libsecret-1-0 libsecret-1-dev make gcc

cd /usr/share/doc/git/contrib/credential/libsecret

sudo make

```

2. Configure Git:

```bash

git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret

```

  

## 3. Clone the Repository

  

You need to clone the remote repository to your local machine.

  

### Option A: HTTPS (Easier)

  

Use this if you are not familiar with SSH keys.

  

```bash

git clone https://github.com/andrezz-b/dtu-salsa-data.git

```

  

> [!WARNING]
> If prompted for a password, you **MUST** use a **Personal Access Token (PAT)**. Your regular GitHub password will **not** work.
>
> 👉 **[Click here to see how to get a PAT](#how-to-get-a-personal-access-token-pat)**
>
> _By cloning successfully, your credentials will be saved by the helper configured in step 2, enabling Obsidian to push changes automatically._

  

### Option B: SSH (Recommended)

  

Use this if you have set up SSH keys with GitHub.

  

```bash

git clone git@github.com:andrezz-b/dtu-salsa-data.git

```

  

_Need help setting up SSH? See [GitHub's SSH documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)._

  

## 4. Open Vault

  

1. Launch **Obsidian**.

2. Click **"Open folder as vault"**.

3. Select the `dtu-salsa-data` folder you just cloned.

4. **Trust**: If asked, confirm you trust the authors (this enables the plugins).

  

**That's it!** The repository comes pre-configured with the necessary plugins (including `obsidian-git`). You should be able to push and pull changes immediately.

  

---

  

## How to get a Personal Access Token (PAT)

  

If you use HTTPS, you need a token instead of a password.

  

1. Log in to GitHub.

2. Go to **Settings** -> **Developer settings** -> **Personal access tokens** -> **Tokens (classic)**.

- _Or use this direct link: [Generate New Token](https://github.com/settings/tokens/new)_

3. Click **Generate new token** -> **Generate new token (classic)**.

4. **Note**: Give it a name like "Obsidian Sync".

5. **Expiration**: Choose "No expiration" (e.g., for convenience) or set a duration.

6. **Select scopes** (Permissions):

- ✅ **`repo`** (Full control of private repositories)

- _This single checkbox covers everything needed to read and write to the repository._

7. Click **Generate token**.

8. **COPY the token immediately**. You won't be able to see it again!

9. Use this token string as your **Password** when Git asks for one.

  

---

  

**References:**

  

- [Obsidian Git: Getting Started](https://publish.obsidian.md/git-doc/Getting+Started)

- [Obsidian Git: Installation](https://publish.obsidian.md/git-doc/Installation)

- [Obsidian Git: Authentication](https://publish.obsidian.md/git-doc/Authentication)
