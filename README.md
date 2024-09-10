# Authenticate Git and GitHub Using SSH
## On Linux, MacOS or WSL
**Step 1: Generate a New SSH Key (If you don't have one)**
If you don’t already have an SSH key, generate one using either the Ed25519 or RSA algorithm. 
1. Open Terminal
2. Generate a new SSH key:

   * Using `ed25519` (preffered for modern use):
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
     
5. Set a passphrase (optional but recommended).<br>
   (It is a password-like string that you can set for your SSH key. It encrypts your private key file, so even if someone gains access to the file, they would still need the passphrase to use it.  You will need to enter a passphrase each time you use the key.)

**Step 2: Add the SSH Key to the SSH Agent**
1. Start SSH Agent:
   
   ```
   eval "$(ssh-agent -s)"
   ```
   
3. Add your SSH key to the agent:
   
   ```
   ssh-add ~/.ssh/id_ed25519
   ```
   
     OR
   
   ```
   ssh-add ~/.ssh/id_rsa
   ```

**Step 3: Automate SSH Agent Setup**

To avoid manually starting the SSH agent every time you open a WSL terminal, you can add the SSH agent startup and key addition commands to your shell configuration file:

1. Edit Your Shell Configuration File (using VS Code):
   
   * For `bash` users:
  
     ```
     code ~/.bashrc
     ```

  
   * For `zsh` users:
  
     ```
     code ~/.zshrc
     ```

3. Add SSH Agent Initialization
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
   source ~/.bashrc
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

3. Log in to **GitHub** and navigate to your account settings:
   * Go to GitHub.
   * Click on your profile picture in the top right corner.
   * Select Settings.
  
4. Add the SSH key to GitHub:
   * In the left sidebar, click on SSH and GPG keys.
   * Click the New SSH key button.
   * In the "Title" field, add a descriptive name for the key.
   * Paste your SSH public key into the "Key" field.
   * Click Add SSH key.
  
5. Confirm your GitHub password if prompted.

**Step 5: Test Your SSH Connection**
1. Test the connection to GitHub:

   ```
   ssh -T git@github.com
   ```

3. You should see a message like:

   ```
   Hi username! You've successfully authenticated
   ```

**Step 6: Configure Git to use SSH**
1. Clone a repository using SSH:<br>Instead of the HTTPS URL `(https://github.com/username/repository.git)`, use the SSH URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

3. Set SSH as the default for an existing repository:<br>If you’ve cloned a repository using HTTPS and want to switch to SSH, update the remote URL:

   ```
   git remote set-url origin git@github.com:username/repository.git
   ```

**Step 7: Using Git with SSH**

Now that SSH is set up, you can perform all your Git operations (clone, pull, push, etc.) securely without needing to enter your GitHub password.
