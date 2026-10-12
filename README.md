Here is the complete, end-to-end master guide for setting up GPG (GNU Privacy Guard) cryptographic commit signing. When configured correctly, every commit you push to GitHub will carry a cryptographic seal and display a green Verified badge next to your commits.
Phase 1: Install GnuPG (GPG)
Ensure GPG is installed on your operating system:
 * macOS (via Homebrew):
   brew install gnupg

 * Linux (Ubuntu / Debian):
   sudo apt update && sudo apt install gnupg

 * Windows (via Winget):
   winget install GnuPG.GnuPG

Phase 2: Generate a New GPG Key
 1. Run the key generation command
   Terminal
   Execute the interactive key generation tool:
   gpg --full-generate-key

 2. Select key parameters
   Interactive Prompts
   Follow the prompts in your terminal:
   * Key type: Press 1 for RSA and RSA (recommended for maximum GitHub compatibility).
   * Key size: Enter 4096 for maximum security.
   * Expiration: Set an expiration (e.g., 0 for no expiration, or 1y for 1 year).
   * Confirmation: Type y to confirm.
   * User ID: Enter your Real Name and the Exact Email Address associated with your GitHub account.
   * Passphrase: Enter a secure passphrase when prompted and store it safely in your password manager (like Bitwarden).
Phase 3: Retrieve Your GPG Key ID
To configure Git, you need your unique GPG Key ID.
 * List your secret keys with long formatting:
   gpg --list-secret-keys --keyid-format=long

 * Look for the output block that resembles this:
   sec   rsa4096/3AA5C34371567BD2 2026-10-12 [SC]
      Key fingerprint = 4B38 97FC 710E 2475 2B5D  8CC4 3AA5 C343 7156 7BD2
uid [ultimate] Your Name <your_email@example.com>
ssb   rsa4096/4F21E5C682A789EF 2026-10-12 [E]

 * Your Key ID is the 16-character alphanumeric string immediately after rsa4096/ (in this example: 3AA5C34371567BD2). Copy your own Key ID.
Phase 4: Export Your Public GPG Key
GitHub requires your public key block to verify your signed commits.
 * Export your public key using your Key ID (replace <YOUR_KEY_ID> with your actual 16-character ID):
   gpg --armor --export <YOUR_KEY_ID>

 * Copy the entire output text block, starting with -----BEGIN PGP PUBLIC KEY BLOCK----- and ending with -----END PGP PUBLIC KEY BLOCK-----.
Phase 5: Add the GPG Key to GitHub
 * Open GitHub in your browser, click your profile picture, and go to Settings.
 * In the left sidebar, click Access > SSH and GPG keys.
 * Click the New GPG key button.
 * Give your key a descriptive title (e.g., Dev Machine GPG), paste your copied public key block into the Key field, and click Add GPG key.
Phase 6: Configure Global Git Settings
Link your GPG key to your local Git environment and enable automatic signing for all commits.
# 1. Set your GPG signing key (replace with your 16-character Key ID)
git config --global user.signingkey 3AA5C34371567BD2

# 2. Force Git to automatically sign all future commits
git config --global commit.gpgsign true

Phase 7: Configure GPG Terminal Pinentry (Critical for CLI)
If you are committing via the terminal, GPG needs to know how to prompt you for your passphrase.
 * Ensure your shell exports the current terminal ID:
   echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
# Or if using zsh:
echo 'export GPG_TTY=$(tty)' >> ~/.zshrc
source ~/.bashrc  # (or source ~/.zshrc)

 * Configure your gpg-agent configuration (~/.gnupg/gpg-agent.conf) to ensure smooth pinentry prompts:
   echo "default-cache-ttl 600" >> ~/.gnupg/gpg-agent.conf
echo "max-cache-ttl 7200" >> ~/.gnupg/gpg-agent.conf
gpgconf --reload gpg-agent

Phase 8: Test Your Setup
Make a test commit and push it to GitHub to verify your verified badge:
# Create a test change, stage, and commit
echo "test" > gpg-test.txt
git add gpg-test.txt
git commit -m "test: verify gpg commit signature"
git push origin main

Go to your repository on GitHub, navigate to the Commits tab, and confirm that your commit displays a green Verified badge.
GnuPG.GnuPG10409624752026-10-17156your_email@example.comuser.signingkeycommit.gpgsigngpg-test.txt# Create a test change, stage, and commit
echo "test" > gpg-test.txt
git add gpg-test.txt
git commit -m "test: verify gpg commit signature"
git push origin main
echo "default-cache-ttl 600" >> ~/.gnupg/gpg-agent.conf
echo "max-cache-ttl 7200" >> ~/.gnupg/gpg-agent.conf
gpgconf --reload gpg-agent
# 1. Set your GPG signing key (replace with your 16-character Key ID)
git config --global user.signingkey 3AA5C34371567BD2

# 2. Force Git to automatically sign all future commits
git config --global commit.gpgsign true
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
# Or if using zsh:
echo 'export GPG_TTY=$(tty)' >> ~/.zshrc
source ~/.bashrc  # (or source ~/.zshrc)
https://www.facebook.com/share/19mJket9jY/https://www.facebook.com/share/19fVX4jqDs/record's and spreadsheets and files and documents and devices key's and developers key's badges and permissions T-Mobile files documents spreadsheets and data breaches and Google accounts and passwords and computers and apps All Google timelines All accounts super backup since and restore # raeeinbagheri.com

Personal site of Raeein Bagheri. Plain HTML, CSS and JavaScript with no build step, hosted on GitHub Pages.

## Structure

```
index.html            Home page (experience, projects, skills, education, contact)
projects/*.html       Project case studies
404.html              Not-found page (uses absolute paths; GitHub Pages serves it for any missing URL)
assets/css/style.css  All styles; color tokens and dark theme at the top
assets/js/            theme-init.js (runs in <head>, prevents theme flash) and script.js
assets/docs/resume.pdf
CNAME                 Custom domain
.nojekyll             Tells GitHub Pages to serve files as-is
```

## Preview locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy

Push to `master`. GitHub Pages publishes the repo root.

## Updating

- **Resume:** replace `assets/docs/resume.pdf`.
- **New project page:** copy `projects/gmc.html`, edit the content, and add a card in the Projects section of `index.html`.
- **Third-party resources:** each page has a Content-Security-Policy `<meta>` tag. Any new external script, font or image host has to be added there or the browser will block it.
