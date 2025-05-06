# Authenticate Git and GitHub Using SSH
SSH (Secure Shell) helps computers talk to each other safely, even over unsafe networks, by creating a protected connection. <br><br>
When you use SSH with GitHub, you don’t have to type your username and password every time you push code. Instead, you set up something called SSH keys—one on your computer and the other on GitHub—to make logging in automatic and secure.
<br><br>
Read this article, [Mastering SSH: Secure Remote Access with Keys and Configurations](https://controlplusblog.hashnode.dev/mastering-ssh-secure-remote-access-with-keys-and-configurations), to learn more about SSH.

## Table of Contents
- [On Linux, MacOS or WSL](#on-linux-macos-or-wsl)
- [On Windows](#on-windows)
  - [Using Git Bash (recommended)](#method-1-using-git-bash-recommended)
  - [Using PowerShell](#method-2-using-powershell)
  - [Using Command Prompt (not recommended)](#method-3-using-command-prompt-not-recommended)

## On Linux, MacOS or WSL
**Step 1: Generate a New SSH Key**

If you don’t already have an SSH key, generate one using either the Ed25519 or RSA algorithm. 
1. Open Terminal
2. Generate a new SSH key:

   * Using `ed25519` (preferred for modern use):
     ```
     ssh-keygen -t ed25519 -C "your_email@example.com"
     ```
   
     OR
   
   * Using RSA:
     ```
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```

   (Replace `"your_email@example.com"` with the email you use for GitHub.)
   
3. Save the key:
   
   * Specify the file location when prompted.
     
     OR
   * Press Enter to accept the default location (`~/.ssh/id_ed25519` or `~/.ssh/id_rsa`).
     
4. Set a passphrase (optional but recommended).
   
   (It is a password-like string that you can set for your SSH key. It encrypts your private key file, so even if someone gains access to the file, they would still need the passphrase to use it.  You will need to enter a passphrase each time you use the key.)

**Step 2: Add the SSH Key to the SSH Agent**
1. Start SSH Agent:
   
   ```
   eval "$(ssh-agent -s)"
   ```
   
2. Add your SSH key to the agent:
   
   ```
   ssh-add ~/.ssh/id_ed25519
   ```
   
     OR
   
   ```
   ssh-add ~/.ssh/id_rsa
   ```

**Step 3: Automate SSH Agent Setup**

To avoid manually starting the SSH agent every time you open a terminal, you can add the SSH agent startup and key addition commands to your shell configuration file:

1. Edit Your Shell Configuration File (using VS Code):
   
   * For `bash` users:
  
     ```
     code ~/.bashrc
     ```

  
   * For `zsh` users:
  
     ```
     code ~/.zshrc
     ```

2. Add SSH Agent Initialization
   ```
   # Start SSH agent
   if ! pgrep -u "$USER" ssh-agent > /dev/null; then
     eval $(ssh-agent -s)
   fi

   # Add SSH key
   ssh-add ~/.ssh/id_ed25519

   # Or for RSA
   # ssh-add ~/.ssh/id_rsa
   ```

3. Save and Exit

4. Apply the changes immediately by running:

   ```
   source ~/.bashrc
   ```

   OR

   ```
   source ~/.zshrc
   ```

   
**Step 4: Add Your SSH Key to Your GitHub Account**
1. Copy your SSH public key:
   
   ```
   cat ~/.ssh/id_ed25519.pub
   ```
   
   OR
   
   ```
   cat ~/.ssh/id_rsa.pub
   ```

   (This command displays your public key in the terminal. Copy the entire output, including the `ssh-ed25519` or `ssh-rsa` prefix.)

2. Log in to **GitHub** and navigate to your account settings:
   * Go to GitHub.
   * Click on your profile picture in the top right corner.
   * Select Settings.
  
3. Add the SSH key to GitHub:
   * In the left sidebar, click on SSH and GPG keys.
   * Click the New SSH key button.
   * In the "Title" field, add a descriptive name for the key.
   * Paste your SSH public key into the "Key" field.
   * Click Add SSH key.
  
4. Confirm your GitHub password if prompted.

**Step 5: Test Your SSH Connection**
1. Test the connection to GitHub:

   ```
   ssh -T git@github.com
   ```

2. You should see a message like:

   ```
   Hi username! You've successfully authenticated
   ```

**Step 6: Configure Git to use SSH**
1. Clone a repository using SSH:<br>Instead of the HTTPS URL `(https://github.com/username/repository.git)`, use the SSH URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

2. Set SSH as the default for an existing repository:<br>If you’ve cloned a repository using HTTPS and want to switch to SSH, update the remote URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

**Step 7: Using Git with SSH**

   Now that SSH is set up, you can perform all your Git operations (clone, pull, push, etc.) securely without needing to enter your GitHub password.

## On Windows

### Method 1: Using Git Bash (recommended)
**Step 1: Generate a New SSH Key** 

If you don’t already have an SSH key, generate one using either the Ed25519 (preferred) or RSA algorithm.

1. Open Git Bash
2. Generate a new SSH key:
     ```
     ssh-keygen -t ed25519 -C "your_email@example.com"
     ```

   (Replace `"your_email@example.com"` with the email you use for GitHub.)
   
3. Save the key:
   
   * Specify the file location when prompted.
     
     OR
   * Press Enter to accept the default location (`~/.ssh/id_ed25519`). Git Bash uses a Unix-style path (`~` is shorthand for your home directory). It points to `C:\Users\<YourUsername>\.ssh\id_ed25519` in Windows system.
     
4. Set a passphrase (optional but recommended).

**Step 2: Add the SSH Key to the SSH Agent**
1. Start SSH Agent:
   
   ```
   eval "$(ssh-agent -s)"
   ```
   
3. Add your SSH key to the agent:
   
   ```
   ssh-add ~/.ssh/id_ed25519
   ```

**Step 3: Automate SSH Agent Setup**

Git Bash doesn’t have built-in persistence for the SSH agent, so you can add the following to your `~/.bashrc` or `~/.bash_profile` file to automatically start the SSH agent and add the key on every session:

  ```
  eval "$(ssh-agent -s)" > /dev/null
  ssh-add ~/.ssh/id_ed25519 2>/dev/null
  ```

**Step 4: Add Your SSH Key to Your GitHub Account**
1. Copy your SSH public key:
   
   ```
   clip < ~/.ssh/id_ed25519.pub
   ```

2. Log in to **GitHub** and navigate to your account settings:
   * Go to GitHub.
   * Click on your profile picture in the top right corner.
   * Select Settings.
  
3. Add the SSH key to GitHub:
   * In the left sidebar, click on SSH and GPG keys.
   * Click the New SSH key button.
   * In the "Title" field, add a descriptive name for the key.
   * Paste your SSH public key into the "Key" field.
   * Click Add SSH key.
  
4. Confirm your GitHub password if prompted.

**Step 5: Test Your SSH Connection**
1. Test the connection to GitHub:

   ```
   ssh -T git@github.com
   ```

2. You should see a message like:

   ```
   Hi username! You've successfully authenticated
   ```

**Step 6: Configure Git to use SSH**
1. Clone a repository using SSH:<br>Instead of the HTTPS URL `(https://github.com/username/repository.git)`, use the SSH URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

2. Set SSH as the default for an existing repository:<br>If you’ve cloned a repository using HTTPS and want to switch to SSH, update the remote URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

**Step 7: Using Git with SSH**

Now that SSH is set up, you can perform all your Git operations (clone, pull, push, etc.) securely without needing to enter your GitHub password.

### Method 2: Using PowerShell
**Step 1: Generate a New SSH Key** 

If you don’t already have an SSH key, generate one using either the Ed25519 (preferred) or RSA algorithm.

1. Open PowerShell
2. Generate a new SSH key:
     ```
     ssh-keygen -t ed25519 -C "your_email@example.com"
     ```

   (Replace `"your_email@example.com"` with the email you use for GitHub.)
   
3. Save the key:
   
   * Specify the file location when prompted.
     
     OR
   * Press Enter to accept the default location (`C:\Users\YourUsername\.ssh\id_ed25519`).
     
4. Set a passphrase (optional but recommended).

**Step 2: Add the SSH Key to the SSH Agent**
1. Start SSH Agent:
   
   ```
   Start-Service ssh-agent
   ```
   
3. Add your SSH key to the agent:
   
   ```
   ssh-add $env:USERPROFILE\.ssh\id_ed25519
   ```

**Step 3: Automate SSH Agent Setup**

To avoid starting the SSH agent and adding the key manually every time, you can automate this in PowerShell. Add the following to your PowerShell profile:

```
notepad $PROFILE
```
Then add:
```
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519 | Out-Null
```

**Step 4: Add Your SSH Key to Your GitHub Account**
1. Copy your SSH public key to your clipboard:
   
   ```
   Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" | Set-Clipboard
   ```

2. Log in to **GitHub** and navigate to your account settings:
   * Go to GitHub.
   * Click on your profile picture in the top right corner.
   * Select Settings.
  
3. Add the SSH key to GitHub:
   * In the left sidebar, click on SSH and GPG keys.
   * Click the New SSH key button.
   * In the "Title" field, add a descriptive name for the key.
   * Paste your SSH public key into the "Key" field.
   * Click Add SSH key.
  
4. Confirm your GitHub password if prompted.

**Step 5: Test Your SSH Connection**
1. Test the connection to GitHub:

   ```
   ssh -T git@github.com
   ```

2. You should see a message like:

   ```
   Hi username! You've successfully authenticated
   ```

**Step 6: Configure Git to use SSH**
1. Clone a repository using SSH:<br>Instead of the HTTPS URL `(https://github.com/username/repository.git)`, use the SSH URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

2. Set SSH as the default for an existing repository:<br>If you’ve cloned a repository using HTTPS and want to switch to SSH, update the remote URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

**Step 7: Using Git with SSH**

Now that SSH is set up, you can perform all your Git operations (clone, pull, push, etc.) securely without needing to enter your GitHub password.


### Method 3: Using Command Prompt (not recommended)
**Step 1: Generate a New SSH Key** 

If you don’t already have an SSH key, generate one using either the Ed25519 (preferred) or RSA algorithm.

1. Open Command Prompt
2. Generate a new SSH key:
     ```
     ssh-keygen -t ed25519 -C "your_email@example.com"
     ```

   (Replace `"your_email@example.com"` with the email you use for GitHub.)
   
3. Save the key:
   
   * Specify the file location when prompted.
     
     OR
   * Press Enter to accept the default location (`C:\Users\YourUsername\.ssh\id_ed25519`).
     
4. Set a passphrase (optional but recommended).

**Step 2: Add the SSH Key to the SSH Agent**
1. Start SSH Agent:
   
   ```
   start-ssh-agent.cmd
   ```
   
3. Add your SSH key to the agent:
   
   ```
   ssh-add %userprofile%\.ssh\id_ed25519
   ```

**Step 3: Automate SSH Agent Setup**

CMD doesn't support persistent agent service, so you must start the agent each session. For persistent key storage across reboots, you must use PowerShell.

**Step 4: Add Your SSH Key to Your GitHub Account**
1. Copy your SSH public key. Since CMD doesn’t have clipboard support, use PowerShell to copy the public key:
   
   ```
   powershell Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" | Set-Clipboard
   ```

2. Log in to **GitHub** and navigate to your account settings:
   * Go to GitHub.
   * Click on your profile picture in the top right corner.
   * Select Settings.
  
3. Add the SSH key to GitHub:
   * In the left sidebar, click on SSH and GPG keys.
   * Click the New SSH key button.
   * In the "Title" field, add a descriptive name for the key.
   * Paste your SSH public key into the "Key" field.
   * Click Add SSH key.
  
4. Confirm your GitHub password if prompted.

**Step 5: Test Your SSH Connection**
1. Test the connection to GitHub:

   ```
   ssh -T git@github.com
   ```

2. You should see a message like:

   ```
   Hi username! You've successfully authenticated
   ```

**Step 6: Configure Git to use SSH**
1. Clone a repository using SSH:<br>Instead of the HTTPS URL `(https://github.com/username/repository.git)`, use the SSH URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

2. Set SSH as the default for an existing repository:<br>If you’ve cloned a repository using HTTPS and want to switch to SSH, update the remote URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

**Step 7: Using Git with SSH**

Now that SSH is set up, you can perform all your Git operations (clone, pull, push, etc.) securely without needing to enter your GitHub password.

## 📬 Connect with Me  
  
<div align="center">

[![X](https://img.shields.io/badge/X-%23000000.svg?logo=X&logoColor=white)](https://twitter.com/VishalKapgate)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?logo=gmail&logoColor=white)](mailto:vishaldk26@gmail.com)
[![LinkedIn](https://custom-icon-badges.demolab.com/badge/LinkedIn-0A66C2?logo=linkedin-white&logoColor=fff)](https://linkedin.com/in/vishalkapgate)
[![Peerlist](https://img.shields.io/badge/-Peerlist-00AA45?style=flat&logo=peerlist&logoColor=white)](https://peerlist.io/vishalkapgate)

</div>

## 🤝 Contributing
Contributions are welcome!  

- Found an error? Let me know (even spelling mistakes count! 📝).  
- Have useful learning notes? Feel free to fork & enhance this repository!
