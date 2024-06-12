+++
title = 'Apply Game Patch on Streamdeck'
date = 2024-06-12T16:35:21+08:00
draft = false
+++
It is quite painful to apply a game patch on Streamdeck. I try to do it by copying files via SSH.


### 1. Enable SSH on the Steam Deck
SSH (Secure Shell) allows secure remote access to the Steam Deck from another machine, such as your Windows PC. 

- **Step 1:** Power on your Steam Deck and access the desktop mode by pressing the Steam button, selecting "Power," and then "Switch to Desktop."
- **Step 2:** Open a terminal window. You can find the terminal in the applications menu.
- **Step 3:** Install the SSH server if it’s not already installed by typing the following command:
  ```bash
  sudo apt-get update
  sudo apt-get install openssh-server
  ```
- **Step 4:** Enable and start the SSH service:
  ```bash
  sudo systemctl enable ssh
  sudo systemctl start ssh
  ```
- **Step 5:** Find your Steam Deck’s IP address by typing:
  ```bash
  ifconfig
  ```
  Look for the `inet` address under the network interface you're using (e.g., `wlan0` for Wi-Fi).

### 2. Obtain the Patched Files and Apply the Patch on the Windows Machine
- **Step 1:** Download the patch files from the official source or the trusted site where the patch is available.
- **Step 2:** Apply the patch to the game files on your Windows machine. This usually involves extracting the patch files and copying them over the existing game files in the installation directory.
- **Step 3:** Locate the path of the patched game files on your Windows machine. This is typically in a directory like `C:\Program Files (x86)\Steam\steamapps\common\` followed by the game's name.

### 3. Create a Tar Archive of the Game Files
Creating a tar archive consolidates many files into one, making it easier and faster to transfer.

- **Step 1:** Open a terminal or command prompt on your Windows machine.
- **Step 2:** Navigate to the directory containing the game files:
  ```cmd
  cd "C:\Program Files (x86)\Steam\steamapps\common\YourGameName"
  ```
- **Step 3:** Use a tool like `7-Zip` to create a tar archive. If `7-Zip` is installed, the command would look like:
  ```cmd
  7z a -ttar gamefiles.tar *
  ```

### 4. Use SCP to Copy the Tar Archive File to the Steam Deck
SCP (Secure Copy Protocol) allows you to securely transfer files between machines.

- **Step 1:** Open a terminal or command prompt on your Windows machine.
- **Step 2:** Use an SCP client like `WinSCP` or the `scp` command if you have an SSH client installed. The command looks like this:
  ```cmd
  scp path\to\gamefiles.tar username@steamdeck_ip_address:/home/deck/
  ```
  Replace `path\to\gamefiles.tar` with the actual path to your tar file, `username` with the username on your Steam Deck (usually `deck`), and `steamdeck_ip_address` with the IP address you found earlier.

### 5. Replace the Original Game Files on the Steam Deck
- **Step 1:** SSH into your Steam Deck from your Windows machine using a terminal or SSH client:
  ```cmd
  ssh username@steamdeck_ip_address
  ```
- **Step 2:** Navigate to the directory where the tar file was copied:
  ```bash
  cd /home/deck/
  ```
- **Step 3:** Locate the game files on the Steam Deck. Typically, the path would be something like `/home/deck/.local/share/Steam/steamapps/common/YourGameName`.
- **Step 4:** Extract the tar archive to replace the original game files:
  ```bash
  tar -xf gamefiles.tar -C /home/deck/.local/share/Steam/steamapps/common/YourGameName --strip-components=1
  ```
  The `--strip-components=1` option removes the first part of the path in the tar archive, ensuring that the files are extracted directly into the game directory.

### Final Steps
- **Step 1:** Ensure that the game files have been replaced correctly.
- **Step 2:** Launch the game from the Steam Deck to verify that the patch has been applied successfully.

By following these steps, you should be able to apply the game patch on your Steam Deck efficiently.