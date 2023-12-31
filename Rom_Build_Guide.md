
#   BUILDING ANDROID FROM SOURCE   #

#  Step One: set up your environment  #

You have two options here, the lazy way or the non-lazy way.

Automatic way (recommended):

Grab a repo with some useful scripts, and run the needed one

      $ sudo apt-get install git-core

      $ git clone https://github.com/akhilnarang/scripts

      $ cd scripts

      $ ls

      $ bash setup/<script-name>

Run the script corresponding to your Linux Distribution:
arch-manjaro.sh - Arch based distros
android_build_env.sh - Ubuntu 14, Ubuntu 16, Ubuntu 18, Mint 19

#  Step Two: Configure Repo and Git  #

NOTE: If you have any problems with the below commands, try running as root:

      $ sudo -s

Then run these commands to get git all working:

      $ git config --global user.name "Your Name"

      $ git config --global user.email "you@example.com"

#  Step Three: Download the source  #

First, create a folder for your source:

      $ mkdir ~/<foldername> (eg. mkdir ~/DU or ~/PN-Layers)

      $ cd ~/<foldername>

When you go to build a ROM, you must download its source. All, if not most,
ROMs will have their source code available on Github. To properly download the
source, follow these steps:

-- Go to your ROM's Github (e.g. http://github.com/DirtyUnicorns)

-- Search for a manifest (usually called manifest or android_manifest).

-- Go into the repo and make sure you are in the right branch (located right
   under the Commits tab).
   
-- Go into the README and search for a repo init command. If one exists, copy
   and paste it into the terminal and hit enter.
   
-- If one does not exist, you can make one with this formula:
   repo init -u <url_of_manifest_repo>.git -b <branch_you_want_to_build>.
   For example:
   
      $ repo init -u https://github.com/DirtyUnicorns/android_manifest.git -b o8x
      
-- After the repo has been initialized, run this command to download the source:

      $ repo sync --force-sync -j$( nproc --all )
   
-- This process can take a while depending on your internet connection.

#  Step Four: Build it!  #

Here's the moment of truth... time to build the ROM! This process could take
15 minutes to many hours depending on the speed of your PC but assuming that
you have followed the instructions so far, it should be smooth sailing.


Before you compile, you may also consider setting up ccache if you can spare
about 50GB. If not, skip this section. ccache is a compiler cache, which keeps
previously compiled code stored so it can be easily reused. This speeds up
build times DRASTICALLY.

Open your bashrc or equivalent:

      $ nano ~/.bashrc

- Append export USE_CCACHE=1 to the end of this file
   then hit ctrl-X, Y, and enter.
  
      $ source ~/.bashrc

After that, run this command if you used the manual method of setup above

      $ prebuilts/misc/linux-x86/ccache/ccache -M 50G (or however much you want).

Run this command if you used the automatic method of setup above

      $ ccache -M 50G

Since Android 10, the way of using ccache has changed:

      $ export USE_CCACHE=1

      $ export CCACHE_EXEC=$(command -v ccache)

After this, load up the compilation commands:

      $ . build/envsetup.sh

Then, tell it which device you want to make and let it roll:

      $ breakfast <device> OR lunch

      $ mka bacon

NOTE: Some ROMs may use their own bacon command, read their manifest as they
will usually outline this.

Others may not use mka, use make -j$( nproc --all )


If you get an error here, make sure that you have downloaded all of the
proper vendor files from your ROM's repository.

Once you tell it mka bacon, the computer will start building. You
will know that it is done when you see a message saying make completed and
telling you where your flashable zip is located.

Whenever you build again, make sure you run a clean build every time by placing
this command in between the other two:

      $ make clobber

That's it! You successfully compiled a ROM from scratch :) By doing this, you
control when you get the new features, which means as soon as they are available
on Github. After this, you may even want to start contributing, one of the
greatest things about Android and open source software in general.

#  Jack issues  #

For those of you who are having jack issues (like saying you ran out of memory),
follow these steps.

Type this into your terminal, substituting the # with how many GBs of RAM
you have:

      $ export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx#g"

Then go into the root of the source folder and type the following:

      $ ./prebuilts/sdk/tools/jack-admin kill-server

      $ ./prebuilts/sdk/tools/jack-admin start-server

This will restart the jack server to reflect your new heap limit.

#  Support  #

If you run into any issues while building, you can use the following chat room for assistance
(be sure to read the rules): https://t.me/AndroidBuildersHelp
This chat is only for build errors or to get the build to compile.

After you have successfully built the ROM, for any required assistance, use this chat
 https://t.me/AndroidBringup
 
#  Source  #

I didnt make this guide by myself but just made some changes go to the link below to get to the original guide.

https://raw.githubusercontent.com/nathanchance/android-tools/main/guides/building_aosp.txt
