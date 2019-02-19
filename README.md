# Qt Widgets example with Conan

This is just a simple application to compile a Qt 5.12 Widgets application using
nothing but Conan and Cmake. You do not need to have QtCreator installed, nor do
you have to have the Qt libraries installed.

# Installing Conan

Install conan to your pip3 home directory. You could install it to the system
folder, but I don't like to do that.

```bash
pip3 install --user setuptools
pip3 install --user wheel
pip3 install --user conan


# Add the bin crafters repository to your conan remote list.
conan remote add bincrafters https://api.bintray.com/conan/bincrafters/public-conan

# list the remotes
conan remote list
```


# Compiling

Before compiling, the only thing we need to make sure we have a few system libs:

```Bash
sudo apt install cmake

# For qt to be able to compile properly.
sudo apt install libxcb-xkb-dev
```

Now we can build the application

```bash
cd SOURCE_CODE_FOLDER

mkdir build

cd build

# Read the conanfile.txt and download any packages that are needed.
# The --build missing flag is set to build any libs that aren't already compiled
# in the repo.
conan install .. --build missing

cmake ..

cmake --build .

```

# Running The application

The shared libraries that the binary
needs are all set in the RPATH, no need to set LD_LIBRARY_PATH

You will however need Qt libraries where to find the plugins and where to find
the fonts (fonts are not included in Qt5.12)

```
cd bin
objdump -x helloworld | grep RUNPATH
```

This should give you something like this...we are looking for the qt library
runpath. In this case, it is the first one. The unique hash might be different
in your run.

``` RUNPATH
/home/gavin/.conan/data/qt/5.12.1/bincrafters/stable/package/0dd713d82134b4dfc5389933ef05d46b22f4e8d1/lib:/home/gavin/.conan/data/glib/2.56.1/bincrafters/stable/package/6d4315d6fa76c4ef3a26fbf347fda7039440232c/lib:/home/gavin/.conan/data/mysql-connector-c/6.1.11/bincrafters/stable/package/5fd09192ba54be6f5a8c375bd7dc093fc61c85a1/lib
```

Once you have the path, you can set the appropriate environment variables to make
the Qt application run.

```Bash
cd bin
export QT_QPA_PLATFORM_PLUGIN_PATH= /home/gavin/.conan/data/qt/5.12.1/bincrafters/stable/package/0dd713d82134b4dfc5389933ef05d46b22f4e8d1/lib/../plugins/platforms
export QT_QPA_FONTDIR=/usr/share/fonts/truetype
./helloworld

```
