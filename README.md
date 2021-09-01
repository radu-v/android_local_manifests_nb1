Follow the steps here: <https://wiki.lineageos.org/devices/bacon/build>, but skip the ccache configuration
----------------------------------------------------------------------------------------------------------

Basically, it's like this:

1. Download required libs
   ```bash
   sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev python
   ```

1. Download repo
   ```bash
   mkdir -p ~/bin && mkdir -p ~/android/lineage && curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo
   ```

1. Put the ~/bin directory in the current user's path of execution
   In recent versions of Ubuntu, ~/bin should already be in your PATH.
   ```bash
   # set PATH so it includes user's private bin if it exists
   if [ -d "$HOME/bin" ] ; then
      PATH="$HOME/bin:$PATH"
   fi
   ```

1. Reload the profile (if the previous step was needed)
   ```bash
   source ~/.profile
   ```

1. Configure git
   ```bash
   git config --global user.email "email@gmail.com" && git config --global user.name "Your Name"
   ```

1. Download the source code:
   ```bash
   cd ~/android/lineage && repo init --depth=1 -u https://github.com/LineageOS/android.git -b lineage-18.1 && repo sync -c --no-tags --no-clone-bundle --force-sync -j8
   ```

1. Download NB1 kernel
   ```bash
   mkdir -p kernel/nokia && git clone https://github.com/GPUCode/android_kernel_nokia_msm8998 kernel/nokia/msm8998
   ```

1. Download NB1 device tree
   ```bash
   mkdir -p device/nokia && git clone https://github.com/GPUCode/device_nokia_NB1 device/nokia/NB1
   ```

1. Download NB1 vendor tree
   ```bash
   mkdir -p vendor/nokia && git clone https://github.com/GPUCode/vendor_nokia_NB1 vendor/nokia/NB1
   ```

1. Build!
   ```bash
   source build/envsetup.sh
   breakfast NB1
   brunch lineage_NB1-eng
   ```

All the steps contain just a single command and can be copied and pasted one after another.

To turn on ccache to speed up build (can put this in .bashrc)
-------------------------------------------------------------
   ```bash
   export USE_CCACHE=1
   export CCACHE_EXEC=/usr/bin/ccache
   ```
   and run `ccache -M 50G` to set max ccache size to 50GB

To update the repo
------------------
   ```bash
   repo sync -c --force-sync --no-tags --no-clone-bundle -j8
   ```

To build a directory
--------------------
   ```bash
   mmma -j8 path
   ```

To build only the kernel
------------------------
   ```bash
   mka bootimage
   ```
