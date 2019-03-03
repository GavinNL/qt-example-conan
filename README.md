# Qt Widgets example with Conan (Default CMake Generator)

This is just a simple application to compile a Qt 5.12 Widgets application using
nothing but Conan and Cmake. You do not need to have QtCreator installed, nor do
you have to have the Qt libraries installed.


# Compiling With Qt Creator

The CMakeLists.txt file was created so that it would work independently of
whether conan was installed. To open and compile this project in QtCreator,
simply open the CMakeLists.txt file and you are ready to go!


# Compiling with Conan

The Conan Package Manager can be used instead of the QtCreator/Qt Libraries installation.
To do this you must first install the Conan Package manager.

## Installing Conan

Install conan to your pip3 home directory. You could install it to the system
folder, but I don't like to do that.

```bash
sudo apt install python3-pip

pip3 install --user setuptools
pip3 install --user wheel
pip3 install --user conan

# Add the bin crafters repository to your conan remote list.
conan remote add bincrafters https://api.bintray.com/conan/bincrafters/public-conan

# list the remotes
conan remote list
```

## Compiling

Before compiling, the only thing we need to make sure we have a few system libs:

```Bash
sudo apt install cmake

# For qt to be able to compile properly.
sudo apt install libxcb-xkb-dev
```

Now we can build the application.

## Conan Generators

Conan uses various generators for generating build script helpers. The
generators that I prefer are `cmake_paths` and `virtualenv`

### Cmake Paths

Using the Cmake Paths generator `-g cmake_paths` will generate a conan_paths.cmake
file. This file should be loaded in as a toolchain file from the command line
so that Cmake knows where to look for the Qt libraries.

```bash
mkdir build && cd build
conan install .. --build missing -g cmake_paths

cmake .. -D CMAKE_TOOLCHAIN_FILE=conan_paths.cmake

cmake --build .
```

### Virtual Environment

The Virtual Environment generator creates a shell script which you can activate
by calling `source activate.sh`. This will allow cmake to find the Qt packages.

```bash
mkdir build && cd build
conan install .. --build missing -g virtualenv

source activate.sh

cmake ..

cmake --build .
```

# Running The application

The shared libraries that the binary
needs are all set in the RPATH, no need to set LD_LIBRARY_PATH

You will however need Qt libraries where to find the plugins and where to find
the fonts (fonts are not included in Qt5.12).

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
