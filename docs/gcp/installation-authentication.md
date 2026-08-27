# Installation & Authentication

## 1. Install Google Cloud SDK (gcloud)

### I. Windows

1. Download the installer: [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)
2. Run the `.exe` and follow the prompts.
3. Restart your terminal.

### II. macOS

```bash
brew install --cask google-cloud-sdk
```

!!! tip
    No Homebrew? Install it first: [https://brew.sh](https://brew.sh)

### III. Linux (Debian/Ubuntu)

```bash
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates gnupg curl
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
sudo apt-get update && sudo apt-get install google-cloud-cli
```

### IV. Verify install (all OS)

```bash
gcloud --version
```

---

## 2. Authenticate

### I. First-time setup wizard

```bash
gcloud init
```

One-time setup wizard. Opens browser login and lets you pick your GCP project. Run once when starting fresh.

### II. Log in for terminal commands

```bash
gcloud auth login
```

Logs **you** in, so you can run `gcloud` commands in the terminal.

### III. Log in for your code/SDK

```bash
gcloud auth application-default login
```

Logs in so **your code/SDK** (like the Gemini SDK) can also access GCP. This is separate from the login above — that one is for you, this one is for your programs.

### IV. Set your project

```bash
gcloud config set project YOUR_PROJECT_ID
```

Replace `YOUR_PROJECT_ID` with your actual GCP project ID.
