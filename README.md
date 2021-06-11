# android_local_manifests_nb1
Local manifests for Nokia 8 (NB1)

Some steps taken from [https://wiki.lineageos.org/devices/bacon/build](https://wiki.lineageos.org/devices/bacon/build)
1. Install needed packages
   ```shell
   sudo apt install bc bison build-essential ccache curl flex g++-multilib \
      gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev \
      lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev \
      libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc \
      zip zlib1g-dev libssl-dev
   ```
1. Create directories
   ```shell
   mkdir -p ~/bin
   mkdir -p ~/android/crDroid
   ```

1. Install the repo command
   ```shell
   curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
   chmod a+x ~/bin/repo
   ```

1. Put the ~/bin directory in your path of execution
   In recent versions of Ubuntu, ~/bin should already be in your PATH.
   ```shell
   # set PATH so it includes user's private bin if it exists
   if [ -d "$HOME/bin" ] ; then
      PATH="$HOME/bin:$PATH"
   fi
   ```
   Then, run source ~/.profile to update your environment.

1. Configure git
   ```shell
   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   ```

1. Initialize the LineageOS source repository
   ```shell
   cd ~/android/crDroid
   repo init -u https://github.com/crdroidandroid/android.git -b 11.0
   ```

1. Clone device tree and vendor by either
   * using local_manifests.xml (not updated)
   > This contains project definitions for the device tree, kernel, customised
   > projects and Proton Clang.
   > ```shell
   > git clone https://github.com/radu-v/android_local_manifests_nb1.git .repo/local_manifests -b lineage-17.1
   > ```
   * cloning each separately
   > ```shell
   > git clone -b lineage-18.1 https://github.com/GPUCode/android_device_nokia_nb1.git device/nokia/NB1
   > git clone -b lineage-18.1 https://github.com/GPUCode/android_device_nokia_msm8998-common.git device/nokia/msm8998-common
   > git clone -b lineage-18.1 https://github.com/GPUCode/proprietery_vendor_nokia.git vendor/nokia
   > git clone -b lineage-18.1 https://github.com/GPUCode/android_kernel_nokia_msm8998/ kernel/nokia/msm8998
   > ```

1. Download the source code
   ```shell
   repo sync -c
   ```

1. source the environment
   `source build/envsetup.sh`
   
1. Turn on ccache to speed up build (can put this in .bashrc)
   ```shell
   export USE_CCACHE=1
   export CCACHE_EXEC=/usr/bin/ccache
   ```
   and run `ccache -M 50G` to set max ccache size

1. to build crDroid
   `lunch NB1 && clear && make -j8`
   or
   `brunch NB1`

1. to only setup the env for building separate modules/directories
   `breakfast NB1`
   
1. to build a directory
   `mmma -j8 path`

1. to build only the kernel
   `m kernel`
