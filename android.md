\#select SDK

ANDROID\_HOME=/home/huynq12/Android/Sdk

or

\#create file local.properties with this type of content

sdk.dir=/home/huynq12/Android/Sdk

\#then run with emulator

react-native run-android



\#accept aggreement

cd ~/Android/Sdk/tools/bin/

./sdkmanager --licenses



\#Cannot run program "/Android/Sdk/build-tools/23.0.1/aapt": error=2, No such file or directory --&gt; Required libraries for 64-bit machines

\#\#ubuntu

sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386

\#\#fedora

sudo yum install zlib.i686 ncurses-libs.i686 bzip2-libs.i686



\#adb: not found

\#\#add ~/Android/Sdk/platform-tools to PATH

