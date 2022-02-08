sudo apt-get -y install make automake cmake curl g++-multilib libtool binutils-gold bsdmainutils pkg-config python3 g++-mingw-w64-x86-64
sudo update-alternatives --config x86_64-w64-mingw32-g++
#set posix type
cd depends
sed -i 's/download.qt.io/master.qt.io/g' packages/qt.mk
sed -i 's/official_releases/archive/g' packages/qt.mk
sudo make HOST=x86_64-w64-mingw32
cd ..
CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site ./configure --with-incompatible-bdb --disable-tests --disable-gui-tests --disable-bench
sudo update-alternatives --config x86_64-w64-mingw32-g++
sed -i 's/gmtime_r(&time_val, &ts);/(gmtime_s(&ts, &time_val) != 0);/g' src/util/time.cpp
make
