![opalKellyAtisSepia](banner.png "The Opal Kelly ATIS Sepia banner")

Opal Kelly ATIS Sepia is an extension to the [Sepia](https://github.com/neuromorphic-paris/sepia) library to read events from an ATIS with an Opal Kelly shield.

# install

Within a Git repository, run the commands:

```sh
mkdir -p third_party
cd third_party
git submodule add https://github.com/neuromorphic-paris/opal_kelly_atis_sepia.git
git submodule update --init --recursive
cd opal_kelly_atis_sepia
sh install.sh
```

The last command requires a direct connection to the Vision Institute local network, since it downloads non-free libraries.

An application using Opal Kelly ATIS must include *third_party/opal_kelly_atis_sepia/resources* to its libraries directory search path, and link to the library opalkellyfrontpanel.

On __Linux__, an application using Opal Kelly ATIS Sepia must link to pthread.

On __macOS__, an application using Opal Kelly ATIS Sepia must run after compiling the command:
```sh
install_name_tool -change\
    libopalkellyfrontpanel.dylib\
    $project/third_party/opal_kelly_atis_sepia/resources/libopalkellyfrontpanel.dylib\
    $application
```
where `$project` is the absolute path to the project directory and `$application` is the absolute path to the compiled executable. If you use [Premake 4.x](https://github.com/premake/premake-4.x), append to the *premake4.lua* script:
```lua
for index, name in ipairs(configurations()) do
    configuration {'macosx', name}
        postbuildcommands {
            'install_name_tool -change libopalkellyfrontpanel.dylib '
            .. path.getabsolute('resources/libopalkellyfrontpanel.dylib')
            .. ' '
            .. path.join(path.join(project().location, name), project().name)}
end
```

# user guides and documentation

User guides and code documentation are held in the [wiki](https://github.com/neuromorphic-paris/opalKellyAtisSepia/wiki).

# contribute

## development dependencies

Opal Kelly ATIS Sepia relies on [Premake 4.x](https://github.com/premake/premake-4.x) (x ≥ 3) to generate build configurations. Follow these steps to install it:
  - __Debian / Ubuntu__: Open a terminal and execute the command `sudo apt-get install premake4`.
  - __macOS__: Open a terminal and execute the command `brew install premake`. If the command is not found, you need to install Homebrew first with the command<br />
  `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`.

[ClangFormat](https://clang.llvm.org/docs/ClangFormat.html) is used to unify coding styles. Follow these steps to install it:
- __Debian / Ubuntu__: Open a terminal and execute the command `sudo apt-get install clang-format`.
- __macOS__: Open a terminal and execute the command `brew install clang-format`. If the command is not found, you need to install Homebrew first with the command<br />
`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`.

## test

To test the library, run from the *opal_kelly_atis_sepia* directory:
```sh
premake4 gmake
cd build
make
cd release
./opal_kelly_atis_sepia
```

After changing the code, format the source files by running from the *opal_kelly_atis_sepia* directory:
```sh
clang-format -i source/opal_kelly_atis_sepia.hpp
clang-format -i test/opal_kelly_atis_sepia.cpp
```

# license

See the [LICENSE](LICENSE.txt) file for license rights and limitations (GNU GPLv3).
