# My Dynamic Starship Shell Prompt

---

| First Created: 2026-07-01 | Last edited: 2026-07-10 | Version: 1.0 |
| ------------------------- | ----------------------- | ------------ |

---

I have developed a custom BASH / Zsh / FISH shell prompt using Starship by extending the functionality of Starship using environment variables and script. The purpose of the customizations are to:

1. Add dynamic theming based upon if the current user is root or not, if the previous command passed or failed, and if the sudo credentials are cached.

2. Add support (at least for my distributions) proper OS module support.

I have tried to make my prompt as easy to install as I can so that beginners to Linux can safely and easily install it (including starship itself) and maintain it.

<div data-block-id="H4FnfqRY" data-callout-type="info" class="callout"><h4 data-block-id="P1Rw6SBr">INFO</h4><ul data-block-id="XqpRX_3Q"><li><p data-block-id="W9_jgUNj" data-spacing="double">All of my customizations take advantage of the currently documented capabilities of Starship, but applied in unique ways. For those with the knowledge (or AI assistance) these scripts can be adapted to other shells.</p></li><li><p data-block-id="qtp0CAO1" data-spacing="single">My theme is set up to be easily modified by adjusting the different Palettes defined in the Starship configuration file.</p></li><li><p data-block-id="PErkYqJa" data-spacing="single">Since my configuration is for all users (including root) I do not modify the <code spellcheck="false">.bashrc</code> under the home directory, but instead I add a configuration file <code spellcheck="false">/etc/profile.d/starship.sh</code> for the BASH shell.</p></li><li><p data-block-id="bfCUP9oc" data-spacing="single">I am storing the<code spellcheck="false"> starship.toml</code> file (and copies thereof) under <code spellcheck="false">/etc/starship</code> directory.</p></li></ul></div>

For BASH, my code goes into a newly created file `/etc/profile.d/starship.sh`. Files in `/etc/profile.d/` are automatically sourced for all users by `/etc/profile` on login shell startup.

For FISH, my code goes into a newly created file called /etc/fish/conf.d/starship.fish that gets executed every time it runs configuration files.

Starship Configuration File: [starship.toml](<./attachments/60991788b2a7c31f-starship.toml> "starship.toml")

## Layout of the Prompt

The following is a display showing the colors used for each pallet and the module names displayed in each field. The actual prompt will contain content, so each module will be longer, shorter, or not displayed at all. Those modules not displayed will still show any bracketing characters around them.

<div data-block-id="81terr7b" data-callout-type="info" class="callout"><h4 data-block-id="LEm5G5Ne">INFO</h4><p data-block-id="jgmJCPO_" data-spacing="double">The below images used to display the Starship modules used and is generated from a spreadsheet, so some artifacts (breaks between colored blocks, misalignments, etc.) are an artifact of how I am generating the display, not how they are rendered in the terminal.</p></div>

![ReadMe-image1.png](<./attachments/46f05a75e33e6319-ReadMe-image1.png>)

<div data-block-id="MKPKlNDM" data-callout-type="info" class="callout"><h4 data-block-id="hJKLT_ky">INFO</h4><p data-block-id="-Q_Xua0n" data-spacing="double">The prompt is the same between all of these themes, the only difference is the value of palette has been changed.</p></div>

### This is what the prompt actually looks like.

**Root User, previous command passed.**

![ReadMe-image2.png](<./attachments/e2b86cec8c50b6f6-ReadMe-image2.png>)

**Root user, previous command faulted.**

![ReadMe-image3.png](<./attachments/df1c9ce278176012-ReadMe-image3.png>)

**Normal user, previous command passed, sudo credentials not cached.**

![ReadMe-image4.png](<./attachments/8f211686e13c8863-ReadMe-image4.png>)

**Normal user, previous command faulted, sudo credentials not cached.**

![image.png](<./attachments/13b8d1cca1a27fcd-image.png>)

**Normal user, previous command passed, sudo credentials cached.**

![image.png](<./attachments/670772a114418fb2-image.png>)

**Normal user, previous command faulted, sudo credentials cached.**

![image.png](<./attachments/8e4636dedc83ab90-image.png>)

## Installation - Ubuntu/openSUSE using BASH shell

These instructions use BASH commands, which should also work in the Zsh shell, but they do not work on the Fish shell. The only instructions in Fish are noted as such (only the last code block) for initializing Starship and my code for Fish. The test distributions are Ubuntu 26.04 LTS and openSUSE Slowroll.

<div data-block-id="4Nvk1A3d" data-callout-type="info" class="callout"><h4 data-block-id="lVhUg4tM">INFO</h4><p data-block-id="Dsye9HBX" data-spacing="double">See the documentation in the Starship guide for other OS's and Shells.</p></div>

<div data-block-id="ZQgI0jVk" data-callout-type="warn" class="callout"><h4 data-block-id="yGlzTDxA">WARN</h4><p data-block-id="bdqZp52Q" data-spacing="double">This installation installs Topgrade, Cargo, and Homebrew in order to maintain Starship. You should run this on a regular basis (at least monthly, if not weekly).</p></div>

### Install one of the Nerd Fonts. I used FiraCode Mono Nerd Font.

(see the link in the References to Nerd Font downloads)

#### Single time install method (not maintained)

If you are not worried about keeping the Nerd Fonts up to date, you can perform the following to install them once. I recommend using the maintained installation method that follows.

```bash
# Create system-wide font directory
sudo mkdir -p /usr/local/share/fonts/NerdFonts

# make sure the tools are installed.
sudo apt install curl unzip

# Download and install FiraCode Nerd Font directly
NERD_FONT_VERSION=$(curl -s https://api.github.com/repos/ryanoasis/nerd-fonts/releases/latest | grep '"tag_name"' | cut -d'"' -f4)
curl -fLo /tmp/FiraCode.zip "https://github.com/ryanoasis/nerd-fonts/releases/download/${NERD_FONT_VERSION}/FiraCode.zip"
sudo unzip -o /tmp/FiraCode.zip -d /usr/local/share/fonts/NerdFonts/
rm /tmp/FiraCode.zip
sudo fc-cache -fv
```

#### Maintained installation method

An alternative way to install and keep updated the FiraCode Mono Nerd font is to use Homebrew. For those using Topgrade, it will update, via Homebrew, the fonts when you run it.

1. Run the following commands, ***one group at a time,*** to install Homebrew if it is not already installed.

> ```bash
> # Install Homebrew
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
> ```
> 
> ```bash
> # Run these commands in your terminal to add Homebrew to your BASH PATH
> echo >> /home/$USER/.bashrc
> echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"' >> /home/$USER/.bashrc
> eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
> ```
> 
> <div data-block-id="ExkgbOMS" data-callout-type="warn" class="callout"><h4 data-block-id="cM2o7NTB">WARN</h4><p data-block-id="uO07VCky" data-spacing="double">Debian based distros and openSUSE based distros have difference commands to install Homebrew's dependencies</p></div>
> 
> ```bash
> # -----------------
> # Debian commands
> # -----------------
> # Install Homebrew's dependencies if you have sudo access
> sudo apt-get install build-essential bubblewrap
> ```
> 
> ```bash
> # -----------------
> # OPENSUSE commands
> # -----------------
> # Install Homebrew's dependencies if you have sudo access
> sudo zypper refresh
> sudo zypper install -y pattern:devel_C_C++ bubblewrap
> ```
> 
> ```bash
> # Test that it is installed
> brew help
> ```
> 
> <div data-block-id="H1k4_fMw" data-callout-type="warn" class="callout"><h4 data-block-id="1zE_4w9v">WARN</h4><p data-block-id="2ddCExDr" data-spacing="double">If you are using Zsh or Fish shells, you will need to add the Homebrew path to those as well from within their shells.</p></div>
> 
> ```Zsh
> # Run these commands in your terminal to add Homebrew to your Zsh PATH
> echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/$USER/.zshrc
> ```
> 
> ```fish
> # Run these commands in your terminal to add Homebrew to your Fish PATH
> echo 'eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv fish)' | sudo tee /etc/fish/conf.d/homebrew.fish
> ```

2. Now install the FiraCode nerd Font Mono using Homebrew and make them available for all users.

> ```bash
> # Install FiraCode Nerd Font via Homebrew
> brew install font-fira-code-nerd-font
> ```
> 
> <div data-block-id="VyHKs5-h" data-callout-type="warn" class="callout"><h4 data-block-id="xgLSQDRb">WARN</h4><p data-block-id="hPuAboZ1" data-spacing="double">Debian based distros and openSUSE based distros have difference commands to Copy fonts to the system-wide font directory</p></div>
> 
> ```bash
> # -----------------
> # UBUNTU commands
> # -----------------
> # Copy fonts to the system-wide font directory
> sudo mkdir -p /usr/local/share/fonts/FiraCodeNerdFont
> echo "Copying Nerd Fonts to System Folder"
> sudo cp ~/.local/share/fonts/FiraCodeNerdFont\*.ttf /usr/local/share/fonts/
> echo "Building System Font Cache"
> sudo fc-cache -fv
> ```
> 
> ```bash
> # -----------------
> # OPENSUSE commands
> # -----------------
> # Copy fonts to the system-wide font directory
> sudo mkdir -p /usr/share/fonts/truetype/FiraCodeNerdFont
> echo "Copying Nerd Fonts to System Folder"
> sudo cp ~/.local/share/fonts/FiraCodeNerdFont*.ttf /usr/share/fonts/truetype/FiraCodeNerdFont/
> echo "Building System Font Cache"
> sudo fc-cache -fs
> ```

#### Using Topgrade to keep your fonts up to date

If you want want keep your fonts up to date automatically I suggest using Topgrade.

1. Since we already installed Homebrew above, I will show how to do it with that to be as distribution agnostic as possible.

> ```bash
> # Installing Topgrade with Homebrew
> brew install topgrade
> ```
> 
> ​ If you did not install Homebrew above here are some other means of doing so:
> 
> ```bash
> # Installing Topgrade with deb-get
> # if needed, here is how to install deb-get.
> sudo apt install curl lsb-release wget
> curl -sL https://raw.githubusercontent.com/wimpysworld/deb-get/main/deb-get | sudo -E bash -s install deb-get
> # now install topgrade with it.
> deb-get install topgrade
> # NOTE: Topgrade will keep deb-get updated by calling deb-get upgrade
> ```
> 
> ```bash
> # Installing Topgrade with opi on openSUSE
> sudo zypper install opi
> opi topgrade
> ```
> 
> ```bash
> # Installing Topgrade with cargo
> cargo install topgrade
> ```

2. Now download the example configuration file ([config.example.toml](<https://github.com/topgrade-rs/topgrade/raw/refs/heads/main/config.example.toml>)) and place it in your config directory with the following name: `~/.config/topgrade.toml`.

> ```bash
> # Copy the example file from the project repository to ~/.config/topgrade.toml 
> # only if the destination file does not already exist,
> mkdir -p ~/.config
> [ ! -f ~/.config/topgrade.toml ]
> curl -L -o ~/.config/topgrade.toml https://github.com/topgrade-rs/topgrade/raw/refs/heads/main/config.example.toml
> ```

3. Open up `~/.config/topgrade.toml` in your favorite text editor and find the `[post_commands]` section and add the below line to that section. This will keep your system wide installation of FiraCode Nerd Fonts up to date by copying them to the system folder after topgrade runs Homebrew and updates them in your user fonts folder.

> <div data-block-id="_v9omrTu" data-callout-type="warn" class="callout"><h4 data-block-id="2fMWZfd4">WARN</h4><p data-block-id="Zxb4l52i" data-spacing="double">Debian based distros and openSUSE based distros have difference commands to Copy fonts to the system-wide font directory</p></div>
> 
> ```toml
> [commands]
> # -----------------
> # UBUNTU commands
> # -----------------
> "Sync FiraCode Nerd Fonts to System" = '''
>   echo "Copying Nerd Fonts to System Folder"
>   sudo cp ~/.local/share/fonts/FiraCodeNerdFont\*.ttf /usr/local/share/fonts/ 
>   echo "Building System Font Cache"
>   sudo fc-cache -fs
>   '''
> ```
> 
> ```toml
> [commands]
> # -----------------
> # OPENSUSE commands
> # -----------------
> "Sync FiraCode Nerd Fonts to System" = '''
>   sudo mkdir -p /usr/share/fonts/truetype/FiraCodeNerdFont
>   echo "Copying Nerd Fonts to System Folder"
>   sudo cp ~/.local/share/fonts/FiraCodeNerdFont*.ttf /usr/share/fonts/truetype/FiraCodeNerdFont/
>   echo "Building System Font Cache"
>   sudo fc-cache -fs
>   '''
> ```

### Install Starship in a way that it is kept up-to-date.

<div data-block-id="XH9KsSPo" data-callout-type="info" class="callout"><h4 data-block-id="dFEjKJMA">INFO</h4><ul data-block-id="xsbF3Agl"><li><p data-block-id="lHSyESTO" data-spacing="double">openSUSE Slowroll: no official zypper package listed; only Tumbleweed and unofficial OBS "home:" repositories (targeting Tumbleweed/Leap, not Slowroll) carry it.</p></li><li><p data-block-id="zSzXL1fI" data-spacing="single">Debian/Ubuntu family: Starship only exists in Debian 13+ and Ubuntu 25.04+ release repositories.</p></li></ul></div>

Below is 4 different methods of installing Starship based upon your needs and comfort level.

<div data-block-id="ODRwUF8q" data-callout-type="hint" class="callout"><h4 data-block-id="LKbso1jk">HINT</h4><p data-block-id="b6_zWrpg" data-spacing="double">For other distributions not listed here see the project repository <a target="_blank" rel="noopener noreferrer nofollow" spellcheck="false" href="https://github.com/starship/starship#-installation">Installation</a> instructions.</p></div>

```bash
# Install for Ubuntu 25.04+ where the package manager keeps it up to date.
sudo apt install starship
```

```bash
# Install for Debian 13+ where the package manager keeps it up to date.
sudo apt install starship
```

```bash
# Install for openSUSE Tumbleweed (see https://software.opensuse.org/package/starship) where 
# the package manager keeps it up to date.
sudo zypper in starship
```

Cargo is the only non‑.deb/.rpm direct download, non‑Snap/Flatpak/AppImage path that works identically across all eight of my tested distributions — Starship has no official native repository package except on openSUSE Tumbleweed (but not Slowroll). See [Starship Issue #545](<https://github.com/starship/starship/issues/545>).

```bash
# Install using Cargo for cross‑distro installs, kept updated by Topgrade + cargo‑update.
# sudo is required to install starship under /usr/local.
sudo cargo install starship --locked --root /usr/local 
# Also install cargo-update so Topgrade can detect it in order to keep starship up-to-date.
cargo install cargo-update  
cargo-install-update -V
```

<div data-block-id="i8oOoivn" data-callout-type="info" class="callout"><h4 data-block-id="XmAb-Mlx">INFO</h4><p data-block-id="CrIJ5xj8" data-spacing="double">Topgrade (ran as your user) updates the system‑wide Starship binary via cargo-update.</p></div>

Use the following commands to copy the `starship.toml` file 7 times to the `/etc/starship/` folder while renaming the files to match the expected names for my configuration.

Open a terminal and navigate to the directory that you downloaded my `starship.toml` file to and run these commands.

```bash
# Copy and rename file to include the palette in the filename,  
sudo mkdir -p /etc/starship
sudo cp "starship.toml" "/etc/starship/starship.toml" 
sudo cp "starship.toml" "/etc/starship/starship_root_error.toml" 
sudo cp "starship.toml" "/etc/starship/starship_root_noerror.toml" 
sudo cp "starship.toml" "/etc/starship/starship_user_error_sudo.toml" 
sudo cp "starship.toml" "/etc/starship/starship_user_error_nosudo.toml" 
sudo cp "starship.toml" "/etc/starship/starship_user_noerror_sudo.toml" 
sudo cp "starship.toml" "/etc/starship/starship_user_noerror_nosudo.toml" 

# Replace the active palette name string with the correct palette for the file.
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'root_error'/" "/etc/starship/starship_root_error.toml"
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'root_noerror'/" "/etc/starship/starship_root_noerror.toml"
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'user_error_sudo'/" "/etc/starship/starship_user_error_sudo.toml"
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'user_error_nosudo'/" "/etc/starship/starship_user_error_nosudo.toml"
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'user_noerror_sudo'/" "/etc/starship/starship_user_noerror_sudo.toml"
sudo sed -i "s/^palette\s*=\s*'[^']*'/palette = 'user_noerror_nosudo'/" "/etc/starship/starship_user_noerror_nosudo.toml"
```

<div data-block-id="xfwaFt9S" data-callout-type="hint" class="callout"><h4 data-block-id="BLzsuy_l">HINT</h4><p data-block-id="oAXNcQHS" data-spacing="double">If you want to change the theme just go under each of the <code spellcheck="false">[palettes.palette]</code> blocks in the original <code spellcheck="false">starship.toml</code> and change the color being referenced and run the above commands again.</p><p data-block-id="L8SGhEb7" data-spacing="single"></p><p data-block-id="JMlgIuqI" data-spacing="single">If you want to change the layout of the prompt, see the Starship documentation at <a target="_blank" rel="noopener noreferrer nofollow" spellcheck="false" href="https://starship.rs/config">Starship Guide: Configuration</a></p></div>

### Configure Starship based upon which shell you are using:

I am implementing dynamical selection of color palettes based upon the exit code of the previous command, user/root user, and whether the sudo credentials are still cached. Currently there is an outstanding feature request for this (opened in late 2023), so I have had to make a workaround. See [Starship issue #5546](<https://github.com/starship/starship/issues/5546>).

Right now, I am the only user so I have not implemented per-user theme change, just a them for the root user and another theme for all of the rest of the users. My code shows (in the code comments) how to implement the per user theme selection if you want that feature.

How this works is that I made a single configuration that has all of the themes (technically color palettes) already coded into the starship.toml file and I make a copy of the file for each theme that I want implemented based upon the criteria stated above. For my needs, that is 6 copies of the file. I then name each of the copies to identify which theme it implements. Then I switch between the different configuration files by 1) hooking into the starship\_palette\_hook functionality of Starship (it calls this function before starting to generate the prompt) and 2) setting which config for it to use by setting the STARSHIP\_CONFIG environment variable with the proper config file location needed for the required theme.

I am also wanting to have a functional [os] module, and what is currently used in Starship is not functional. So I am implementing a workaround for this as well by setting custom environment variables STARSHIP\_OS\_NAME, STARSHIP\_OS\_VERSION, STARSHIP\_OS\_EDITION, STARSHIP\_OS\_CODENAME, and STARSHIP\_OS\_TYPE to the correct values. Each distribution includes this information in the /etc/os-release file, though not all include all of this information. See [Starship Issue #6902](<https://github.com/starship/starship/issues/6902>).

Finally, I am also implementing logging, storing the log at /var/log/starship/starship.log, and if you want a per user storage of the log a commented out example is provided.

Below is how i configured the BASH, FISH, and Zsh configuration to call my code and initialize Starship for all users (including root).

<div data-block-id="K8INR8Lc" data-callout-type="warn" class="callout"><h4 data-block-id="V_uO-nUm">WARN</h4><p data-block-id="e40r3lF7" data-spacing="double">These commands edit system-wide shell config files that affect all accounts, including root.</p></div>

<div data-block-id="0a2dzPbJ" data-callout-type="info" class="callout"><h4 data-block-id="QW05JWnu">INFO</h4><p data-block-id="Nr373REK" data-spacing="double">You should adjust the <code spellcheck="false">case</code> block in the following code blocks if you find your distributions symbol missing or if you want to use another one.</p></div>

#### For the BASH shell, add the following code to the BASH configuration file

<div data-block-id="JDJIm86e" data-callout-type="info" class="callout"><h4 data-block-id="8z0CzTUm">INFO</h4><p data-block-id="2eH9jg3H" data-spacing="double">For BASH, place this code in the <code spellcheck="false">/etc/profile.d/starship.sh</code> configuration file. You must create this file.</p></div>

```bash
# ==============================
# Portable Bash Starship script
# (Debian-based + openSUSE)
# ==============================

# Load os-release (Debian/openSUSE portable)
if [ -r /etc/os-release ]; then
  . /etc/os-release
elif [ -r /usr/lib/os-release ]; then
  . /usr/lib/os-release
fi

# Set the environment variables for Starship [OS] block
export STARSHIP_OS_NAME="$NAME"
export STARSHIP_OS_VERSION="$VERSION_ID"
export STARSHIP_OS_EDITION="$VERSION"
export STARSHIP_OS_CODENAME="${VERSION_CODENAME:-Unknown}"
export STARSHIP_OS_TYPE="$ID_LIKE"

case "$ID" in
  "anduinos")     export STARSHIP_OS_SYMBOL=" " ;;
  "kubuntu")      export STARSHIP_OS_SYMBOL="🎯 " ;;
  "neon")         export STARSHIP_OS_SYMBOL=" " ;;
  "ubuntu")       export STARSHIP_OS_SYMBOL="🎯 " ;;
  "linuxmint")    export STARSHIP_OS_SYMBOL=" " ;;
  "mxlinux"|"mx") export STARSHIP_OS_SYMBOL=" " ;;
  "pop-os")       export STARSHIP_OS_SYMBOL="🍭 " ;;
  "opensuse-slowroll"|"opensuse-tumbleweed"|"opensuse-leap")
                  export STARSHIP_OS_SYMBOL="🦎 " ;;
esac

# Set the default Starship configuration file
export STARSHIP_CONFIG="/etc/starship/starship.toml"

# Define the function Starship should call before it begins generating the prompt
function starship_palette_hook(){
  local exit_code=$STARSHIP_CMD_STATUS
  if [ "$USER" = "root" ]; then
      # user is the root user
      if [ "$exit_code" -ne 0 ]; then
          # last command returned an error code
          export STARSHIP_CONFIG="/etc/starship/starship_root_error.toml"
      else
          # last command did not return an error code
          export STARSHIP_CONFIG="/etc/starship/starship_root_noerror.toml"
      fi
  else
      # user is not the root user
      if [ "$exit_code" -ne 0 ]; then
          # last command returned an error code
          if sudo -n true 2>/dev/null; then
              # sudo credentials are cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_error_sudo.toml"
          else
              # sudo credentials are not cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_error_nosudo.toml"
          fi
      else
          # last command did not return an error code
          if sudo -n true 2>/dev/null; then
              # sudo credentials are cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_noerror_sudo.toml"
          else
              # sudo credentials are not cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_noerror_nosudo.toml"
          fi
      fi
  fi
}

# Install the hook into Starship to call the above function before generating the prompt
starship_precmd_user_func="starship_palette_hook"

# Initialize Starship for generating the prompt
eval "$(starship init bash)"
```

#### For the Zsh shell, add the following code to the Zsh configuration file

<div data-block-id="ILpk3Elz" data-callout-type="info" class="callout"><h4 data-block-id="wjLT_c4g">INFO</h4><p data-block-id="pDyYyyHP" data-spacing="double">For Zsh on openSUSE, place this code at the end of <code spellcheck="false">/etc/zshrc</code> configuration file</p><p data-block-id="LWOljvuz" data-spacing="single">For Zsh on Debian/Ubuntu based distributions, place this code at the end of <code spellcheck="false">/etc/zsh/zshrc</code> configuration file</p></div>

```bash
# ==============================
# Portable Zsh Starship script
# (Debian-based + openSUSE)
# ==============================

# Load os-release (Debian/openSUSE portable)
if [ -r /etc/os-release ]; then
  . /etc/os-release
elif [ -r /usr/lib/os-release ]; then
  . /usr/lib/os-release
fi

# Set the environment variables for Starship [OS] block
export STARSHIP_OS_NAME="$NAME"
export STARSHIP_OS_VERSION="$VERSION_ID"
export STARSHIP_OS_EDITION="$VERSION"
export STARSHIP_OS_CODENAME="${VERSION_CODENAME:-Unknown}"
export STARSHIP_OS_TYPE="$ID_LIKE"

case "$ID" in
  "anduinos")     export STARSHIP_OS_SYMBOL=" " ;;
  "kubuntu")      export STARSHIP_OS_SYMBOL="🎯 " ;;
  "neon")         export STARSHIP_OS_SYMBOL=" " ;;
  "ubuntu")       export STARSHIP_OS_SYMBOL="🎯 " ;;
  "linuxmint")    export STARSHIP_OS_SYMBOL=" " ;;
  "mxlinux"|"mx") export STARSHIP_OS_SYMBOL=" " ;;
  "pop-os")       export STARSHIP_OS_SYMBOL="🍭 " ;;
  "opensuse-slowroll"|"opensuse-tumbleweed"|"opensuse-leap")
                  export STARSHIP_OS_SYMBOL="🦎 " ;;
esac

# Set the default Starship configuration file
export STARSHIP_CONFIG="/etc/starship/starship.toml"

# zsh precmd hook: runs before each prompt, sets STARSHIP_CONFIG
precmd() {
  local exit_code=$STARSHIP_CMD_STATUS
  if [ "$USER" = "root" ]; then
      # user is the root user
      if [ "$exit_code" -ne 0 ]; then
          # last command returned an error code
          export STARSHIP_CONFIG="/etc/starship/starship_root_error.toml"
      else
          # last command did not return an error code
          export STARSHIP_CONFIG="/etc/starship/starship_root_noerror.toml"
      fi
  else
      # user is not the root user
      if [ "$exit_code" -ne 0 ]; then
          # last command returned an error code
          if sudo -n true 2>/dev/null; then
              # sudo credentials are cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_error_sudo.toml"
          else
              # sudo credentials are not cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_error_nosudo.toml"
          fi
      else
          # last command did not return an error code
          if sudo -n true 2>/dev/null; then
              # sudo credentials are cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_noerror_sudo.toml"
          else
              # sudo credentials are not cached
              export STARSHIP_CONFIG="/etc/starship/starship_user_noerror_nosudo.toml"
          fi
      fi
  fi
  echo "from /etc"
}

# Initialize Starship for generating the prompt
eval "$(starship init zsh)"
```

#### For the FISH shell, add the following code to the Fish configuration file

<div data-block-id="XYb2Bj1O" data-callout-type="info" class="callout"><h4 data-block-id="SL-p1TLs">INFO</h4><p data-block-id="75O0QGAl" data-spacing="double">For FISH, place this code at the end of <code spellcheck="false">/etc/fish/config.fish</code> configuration file.</p></div>

```bash
# ==============================
# Portable Fish Starship script
# (Debian-based + openSUSE)
# ==============================

# Load os-release (Debian/openSUSE portable) using fish built-ins
set -l os_release_file ""

if test -r /etc/os-release
    set os_release_file /etc/os-release
else if test -r /usr/lib/os-release
    set os_release_file /usr/lib/os-release
end

if test -n "$os_release_file"
    # Helper: get value for a given key from os-release
    function __osrel_get --no-scope-shadowing
        set -l key $argv[1]
        string match -r "^$key=.*" < $os_release_file | \
        string replace -r "^$key\\s*=(.*)" '$1' | \
        string trim -c '\'"'
    end

    set -gx NAME            (__osrel_get NAME)
    set -gx VERSION_ID      (__osrel_get VERSION_ID)
    set -gx VERSION         (__osrel_get VERSION)
    set -gx VERSION_CODENAME (__osrel_get VERSION_CODENAME)
    set -gx ID_LIKE         (__osrel_get ID_LIKE)
    set -gx ID              (__osrel_get ID)
end

# Set the environment variables for Starship [OS] block
set -gx STARSHIP_OS_NAME $NAME
set -gx STARSHIP_OS_VERSION $VERSION_ID
set -gx STARSHIP_OS_EDITION $VERSION
set -gx STARSHIP_OS_CODENAME $VERSION_CODENAME
set -gx STARSHIP_OS_TYPE $ID_LIKE

switch $ID
case anduinos
    set -gx STARSHIP_OS_SYMBOL " "
case kubuntu
    set -gx STARSHIP_OS_SYMBOL "🎯 "
case neon
    set -gx STARSHIP_OS_SYMBOL " "
case ubuntu
    set -gx STARSHIP_OS_SYMBOL "🎯 "
case linuxmint
    set -gx STARSHIP_OS_SYMBOL " "
case mxlinux mx
    set -gx STARSHIP_OS_SYMBOL " "
case pop-os
    set -gx STARSHIP_OS_SYMBOL "🍭 "
case opensuse-slowroll opensuse-tumbleweed opensuse-leap
    set -gx STARSHIP_OS_SYMBOL "🦎 "
end

# Set the default Starship configuration file
#set -gx STARSHIP_CACHE "/var/log/starship/starship.log"
set -gx STARSHIP_CONFIG "/etc/starship/starship.toml"

# Define the function Starship should call before it begins generating the prompt
function starship_palette_hook --on-event fish_postexec
    set exit_code $status
    if test "$USER" = "root"
        # user is the root user
        if test $exit_code -ne 0
            # last command returned an error code
            set -gx STARSHIP_CONFIG "/etc/starship/starship_root_error.toml"
        else
            # last command did not return an error code
            set -gx STARSHIP_CONFIG "/etc/starship/starship_root_noerror.toml"
        end
    else
        # user is not the root user
        if test $exit_code -ne 0
            # last command returned an error code
            if sudo -n true 2>/dev/null
                # sudo credentials are cached
                set -gx STARSHIP_CONFIG "/etc/starship/starship_user_error_sudo.toml"
            else
                # sudo credentials are not cached
                set -gx STARSHIP_CONFIG "/etc/starship/starship_user_error_nosudo.toml"
            end
        else
            # last command did not return an error code
            if sudo -n true 2>/dev/null
                # sudo credentials are cached
                set -gx STARSHIP_CONFIG "/etc/starship/starship_user_noerror_sudo.toml"
            else
                # sudo credentials are not cached
                set -gx STARSHIP_CONFIG "/etc/starship/starship_user_noerror_nosudo.toml"
            end
        end
    end
end

# Initialize Starship for generating the prompt
starship init fish | source
```

## References

The following sites were helpful in the installation and development of my prompt.  
[Starship Guide](<https://starship.rs/guide/>)[Starship Guide: Configuration](<https://starship.rs/config>)  
[Starship GitHub Repository](<https://github.com/starship/starship>)  
[Nerd Fonts: Downloads](<https://www.nerdfonts.com/font-downloads>)  
[Nerd Fonts: Cheat Sheet](<https://www.nerdfonts.com/cheat-sheet>)  
[EZPrompt Website](<https://ezprompt.net/>)  
[Bash Prompt Generator Site](<https://bash-prompt-generator.org/>)  
[HTML Color Codes Website](<https://htmlcolorcodes.com/color-names/>)  
[Topgrade GitHub Repository](<https://github.com/topgrade-rs/topgrade>) [Topgrade Cargo Crate Website](<https://crates.io/crates/topgrade>) [Cargo GitHub Repository](<https://github.com/rust-lang/cargo>) [Homebrew Install GitHub Repository](<https://github.com/Homebrew/install>) [Homebrew Website](<https://brew.sh/>) [deb-get Website](<https://github.com/wimpysworld/deb-get>)

<div data-block-id="gb3jLEjC" data-callout-type="info" class="callout"><h4 data-block-id="pWi0oA4Q">INFO</h4><p data-block-id="m7TB6R3o" data-spacing="double">Starship is © 2019-present Starship Contributors, ISC licensed. See <a target="_blank" rel="noopener noreferrer nofollow" spellcheck="false" href="https://github.com/starship/starship">https://github.com/starship/starship</a></p></div>

## License

Copyright © 2026 [MyDynamicShellPrompt@thx.email](<mailto:mydynamicshellprompt@thx.email>)

This project is [MIT](<LICENSE>) licensed.

File: [LICENSE.txt](<./attachments/49d53fab0854c99d-LICENSE.txt> "LICENSE.txt")
