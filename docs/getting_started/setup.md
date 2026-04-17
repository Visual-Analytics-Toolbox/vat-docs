# NaoTH Development Setup

## SmartGit

SmartGit is a git-client used by our team. It can be used for free for non-profit and academic projects.

- Download SmartGit:
    - <https://www.syntevo.com/smartgit/>
- Register an academic license: (e.g., use your university email address)
    - <https://www.syntevo.com/register-non-commercial/#academic>
  

## Prerequisites

You need to install a bunch of software before being able to develop code for the NAO robot.

=== "Linux"

    In Linux you need the following dependencies: cmake, gcc and g++ compiler, zlib, qtcreator, git, gettext, java, netbeans, libreadline-dev.

    ??? "NOTE: Cmake 4.x seems to raise compiler error, use 3.x"
        - download latest 3.x release: [https://cmake.org/download/](https://cmake.org/download/)
        - check the PATH environment variable, if it contains `~/.local/bin`
            - `echo $PATH | grep ".local/bin"`
            - it should be before the system paths
        - install  or copy it to `~/.local/`
    
    !!! NOTE
    
        * `gettext` is needed for compiling glib  
        * `libreadline-dev` is needed for our LUA experiments.  
        * We currently supported openjdk version is 21 (LTS). Other versions migth work but are not guaranteed to.
        * For the VideoAnalyzer-Dialog in RobotControl it is potentially necessary to install `ffmpeg-compat-55` package (Arch) see
            [here](https://wiki.archlinux.org/index.php/java#JavaFX.27s_MediaPlayer_constructor_throws_an_exception)

    You can install them in Ubuntu with this:
    ```sh
    # java: currently supported openjdk version is 21 (LTS).
    # Other versions migth work but are not guaranteed to.
    sudo apt install default-jdk openjfx netbeans

    # c++ compile
    sudo apt install build-essential cmake -y
    # c++ cross-compilation (for robots)
    sudo apt install clang llvm lld -y
    # c++ essential libs
    sudo apt install zlib1g-dev uuid-dev -y
    # libreadline-dev is needed for our LUA experiments
    sudo apt install libreadline-dev -y
    # code versioning control
    sudo apt install git
    # needed for compiling glib
    sudo apt install gettext -y
    # for archives in the toolchain repo
    sudo apt install unzip -y
    # for creating an ubuntu image for the nao
    sudo apt install pigz debootstrap -y
    ```

    ??? "Optional Step (Cmake GUI)"
        You may want to install the cmake GUI for easier development. For example with `apt install cmake-curses-gui` after
        installation run ccmake instead of cmake.

        You can also use the QT GUI by installing `sudo apt install cmake-qt-gui` and running cmake-gui after.
    
    ??? "Optional Step (Clang setup)"
        If you want to use clang instead of gcc you need to install the `clang`, `llvm` and `lld` packages. 
        The following snippets are specific for Ubuntu-20.04. For other operating systems they need to be modified.

        In Ubuntu 20.04 clang-12 is installed with the above command however the binaries are not recognized without the version
        suffix. To fix this setup the symlinks for the commands like this.
        ```
        sudo ln -sf /usr/bin/clang-12 /usr/bin/clang
        sudo ln -sf /usr/bin/clang++-12 /usr/bin/clang++
        sudo ln -sf /usr/bin/llvm-ar-12 /usr/bin/llvm-ar
        ```
        You can also install clang 13, 14 and 15 like this:
        ```
        wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -

        sudo add-apt-repository "deb http://apt.llvm.org/focal/ llvm-toolchain-focal-13 main"
        sudo add-apt-repository "deb http://apt.llvm.org/focal/ llvm-toolchain-focal-14 main"
        sudo add-apt-repository "deb http://apt.llvm.org/focal/ llvm-toolchain-focal-15 main"
        
        sudo apt update --allow-insecure-repositories
        
        sudo apt install clang-13 lld-13 llvm-13
        sudo apt install clang-14 lld-14 llvm-14
        sudo apt install clang-15 lld-15 llvm-15
        ```
        After this setup the symlinks as done above. This documentation is taken from [https://apt.llvm.org/](https://apt.llvm.org/)

    ??? "Optional Step (QtCreator)"
        If you want to use QtCreator as code editor, install
        ```
        sudo apt install qtcreator qt4-qmake libqt4-dev 
        ```
        You can use your favorite code editor (VSCode, Kate, ...) instead.


=== "Windows"

    **Visual Studio**  
      - Install Microsoft Visual Studio 2019 Community Edition from [https://visualstudio.microsoft.com/vs/community/](https://visualstudio.microsoft.com/vs/community/) 

      - **Note:** The project is optimized for VS2019 it is therefore recommended to use it.
      - **Configure:** After a fresh installation you should adjust following configurations
        - change tab size and indent size to 2 and enable "Insert Spaces" here  ``Tools->Options->Text Editor->C++->Tabs``
        - enable the "Build"-toolbar (right klick on the top toolbar) 

    **Cygwin**  

      - Get [cygwin](http://www.cygwin.com)
      - Install cygwin, for example to `C:\cygwin`  
        - **Required**: Select the package *make* for installation
        - **Required**: Install the *mintty: Terminal emulator with native Windows look and feel* shell package.  
        - **Optional**: To add mintty to the windows context menu you have to install the cygwin chere package and execute 
          `chere -i -t mintty -s bash` in  a cygwin console with admin privileges. You are now able to right click and open a mintty bash console in any folder.

    **Java**  
    Install Java JDK 21 from https://learn.microsoft.com/de-de/java/openjdk/download#openjdk-21
    
    **NetBeans**  

      - Get [NetBeans](https://netbeans.apache.org/front/main/download/) and install it. Currently we tested our setup with Netbeans 21.
      - **Important**: Make sure that your Java Path is properly set before installing, otherwise it won't work!  

    **Clang**

    You can use clang for cross compiling for the NAO. For that install a recent LLVM version from [here](https://github.com/llvm/llvm-project/releases). 
    For example choose [LLVM-14.0.6-win64.exe](https://github.com/llvm/llvm-project/releases/download/llvmorg-14.0.6/LLVM-14.0.6-win64.exe)

## Toolchain Setup
=== "Linux"

    Clone the toolchain repo either from our [internal gitlab](https://scm.cms.hu-berlin.de/berlinunited/tools/linuxtoolchain) (active development):
    ```sh
    git clone --depth 1 https://scm.cms.hu-berlin.de/berlinunited/tools/linuxtoolchain toolchain
    ```
    or [public github](https://github.com/BerlinUnited/linuxtoolchain) repo (yearly release):
    ```sh
    git clone --depth 1 https://github.com/BerlinUnited/linuxtoolchain toolchain
    ```
    This will clone the latest version to the `toolchain` directory. If you want the full git history, omit the `--depth 1` argument.


    ??? "Speed up compilation"
        You can speed up the compilation of the toolchain by setting the `MAKEFLAGS` environment variable:
        ```
        export MAKEFLAGS="-j $(nproc)"
        ```
        This is also useful for the project compilation, since it utilizes all available CPUs. 
        Therefore, the above line have to be added to the `~/.profile` and/or `~./.bashrc` file.  
        

    After cloning, execute `CMAKE_BUILD_PARALLEL_LEVEL=12 ./setup.sh -j 12` to compile and install the required libraries.
    
    __Note__: If you want to use the default path configuration in `~/.profile` and/or `~/.bashrc` you have to restart your computer because this file is only read by the login shell and/or your session/window manager during start up.

    ! Now copy the (newly generated) file projectconfig.user.lua to /path/to/NaoTHSoccer/Make/.
    
    TODO: add missing parts here

    TODO: how to compile NaoSmal??? You need the naoqi toolchain in AL_DIR

=== "Windows"

    Clone the toolchain repo from either our [internal gitlab](https://scm.cms.hu-berlin.de/berlinunited/tools/windowstoolchain)
    or [public github](https://github.com/BerlinUnited/windowstoolchain). In this docu to the path of the cloned repo is called
    `<NaoTH-Projekt/toolchain>` Obviously on your system the path looks different.

    Both repos are synced but the development only happens in the internal repo.

    Inside `<NaoTH-Projekt/NaoTHToolchain>` run `setup.bat` as admin.

    This script will generate `projectconfig.user.lua` which must be copied `<NaoTH-Projekt/Naoth-2020>/NaoTHSoccer/Make`.
    This is later used for configuring the compile step if needed.

    TODO: how to compile NaoSmal??? You need the naoqi toolchain in AL_DIR


## Clone and build
To work with the project, checkout the git repo:
```sh
git clone <url of this repo> <NaoTH-Projekt/Naoth-2020>
```
where `<NaoTH-Projekt/Naoth-2020>` is the desired path to the repository on your machine.

=== "Linux"

    1. TODO

=== "Linux Crosscompilation"

    There are two ways to compile the naoth source code for the NAO. First you can target the softbank provided
    image for the NAO or use clang and target the self build Ubuntu image.

    #### Softbank Image
    In order to build the naoth binary go to `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make` and run `./compileGame.sh -j 4`

    #### Clang
    To tell premake to use the ubuntu toolchain you need to change two lines in `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make/projectconfig.user.lua`
    
    (If `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make/projectconfig.user.lua` does not exist, you did not do section setup toolchain correctly).

    ```lua
    NAO_CTC="<NaoTH-Projekt/NaoTHToolchain>/toolchain_nao_ubuntu/"
    if PLATFORM == "Nao" then
        _OPTIONS["crosscompiler"] = "clang"
    end
    ```


    In order to build the naoth binary go to `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make` and run `./compileGame.sh -j 4`

    TODO: mention lolaadaptor and naosmal compilation

=== "Windows"

    Go to the `<NaoTH-Projekt/Naoth-2020>/NaoTHSoccer/Make` directory and execute `generate-vs2019.bat` to 
    create Visual Studio 2019 project files `<NaoTH-Projekt/Naoth-2020>/NaoTHSoccer/build/vs2019`
    
    Open the NaoTHSoccer.sln file with Visual Studio and you have all the projects loaded that are needed for compilation.
    You can now build each project by right clicking on it and then clicking on build.

=== "Windows Crosscompilation"

    There are two ways to compile the naoth source code for the NAO. First you can use cygwin and target the softbank provided
    image for the NAO or use clang and target the self build Ubuntu image.

    #### Cygwin + Softbank Image
    You need to have cygwin installed as described above in the prerequisites section.

    In order to build the naoth binary go to `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make`, open the mintty bash console in
    this folder via the context menu and run `./compileGame.sh -j 4`

    #### Clang + Ubuntu Image
    You need to have cygwin and LLVM installed as described above in the prerequisites section.

    To tell premake to use the ubuntu toolchain you need to change two lines `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make/projectconfig.user.lua`  

    - NAO_CTC=<NaoTH-Projekt/NaoTHToolchain>/toolchain_nao_ubuntu  
    - _OPTIONS["crosscompiler"] = "clang"

    In order to build the naoth binary go to `<NaoTH-Projekt>/Naoth-2020/NaoTHSoccer/Make`, open the mintty bash console in
    this folder via the context menu and run `./compileGame.sh -j 4`

    TODO: mention lolaadaptor and naosmal compilation

---
For development inside WSL follow the linux instructions.

## Additional tools for development
We use different tools with our project:

=== "Linux"

    **Clangd LSP for NaoTHSoccer**
    Use this if you do not want to install the proprietary Microsoft C++ plugin for VS Code(ium) or you do not use VS at all.

    ! Only works on Linux+Mac, as [Bear](https://github.com/rizsotto/Bear) does not work on Windows. On Windows, use VS for now.

    0. Install [Bear](https://github.com/rizsotto/Bear) e.g. `sudo apt install bear`
    1. ! Delete the build/ directory !
    2. cd Make/
    3. `bear -- ./compileGame.sh -j 12`
    4. `mv compile_commands.json /path/to/naoth-2020`

    **XabslEditor**  
    We have a dedicated editor for editing and compiling the robots behavior written in xabsl.

      - clone: `git clone https://github.com/BerlinUnited/xabsleditor.git`  
      - load the Netbeans project from `xabsleditor` and run the application  
      - manual (after the project was loaded once with netbeans):  
        - change dir `cd xabsleditor`  
        - compile: `./gradlew clean build`
        - run: `./gradlew run` or `./dist/xabsleditor` or `java -jar dist/lib/xabsleditor-1.2.jar`  
     
    **RobotControl**  

      - For controlling, modify and analyze the nao robot
      - load the Netbeans project `RobotControl` and run the application
        - File->Import Project `<ProjectDir>`/RobotControl
      - manual:
        - compile: `./gradlew clean build`
        - run: `./gradlew run` or `./dist/robotcontrol` or `java -jar dist/lib/RobotControl.jar`
     
    **NaoSCP**  
      See NaoSCP Documentation
     
    **Simspark**  

      - to run a simulated version of our naoth project  
      - check the [Simspark Setup Guide ](Simspark-Linux-Setup) for installation instructions  
     
    **Python**
      For working with logfiles we have a set of python scripts in the `utils/py` folder. The basic functionality is inside a the naoth python package which you can install with pip:    

       - run `python3 -m pip install -e naoth` in the `<repository>/Utils/py` this will install protobuf as well 

=== "Windows"

    **XabslEditor**  
    We have a dedicated editor for editing and compiling the robots behavior written in xabsl.

      - clone: `git clone https://github.com/BerlinUnited/xabsleditor.git`  
      - load the Netbeans project from `xabsleditor` and run the application  
      - manual (after the project was loaded once with netbeans):  
        - change dir `cd xabsleditor`  
        - compile: `./gradlew.bat clean build`  
        - run: `./gradlew.bat run` or `./dist/xabsleditor.bat` or `java -jar dist/lib/xabsleditor-1.2.jar`  
     
    **RobotControl**  

      - For controlling, modify and analyze the nao robot
      - load the Netbeans project `RobotControl` and run the application
        - File->Import Project `<ProjectDir>`/RobotControl
      - manual:
        - compile: `./gradlew.bat clean build`
        - run: `./gradlew.bat run` or `./dist/robotcontrol.bat` or `java -jar dist/lib/RobotControl.jar`
     
    **NaoSCP**  
      See NaoSCP Documentation
     
    **Simspark**  

      - to run a simulated version of our naoth project  
      - check the [Simspark Setup Guide ](Simspark-Windows-Setup) for installation instructions  
     
    **Python**   
      For working with logfiles we have a set of python scripts in the `utils/py` folder. The basic functionality is inside a the naoth python package which you can install with pip:    

       - run `pip install -e naoth` in the `<repository>/Utils/py` this will install protobuf as well 

## Known Issues
### Premake
**Premake5 is not found**  

  - can happen if your PATH is exceeding the 1024 character limit if PATH is set with setup.bat
  - Solution add it manualy to the Path. Note that trying to append to PATH with the setup.bat script can delete the 
    last characters until the string that should be appended fits. In this case other paths are corrupted and don't work anymore.

### Java
**RobotControlGUI, NaoSCP and the XabslEditor don't scale correctly for high resolution displays**

  - to fix this install java jdk 11+
  - OR pass a scaling flag to the app: `GDK_SCALE=1.5 ./gradlew run`

**RobotControl Dialogs can't be created/opened**

  The Java Platform Module System might isolate components too much.
  You can try to add following options to the `java` command when starting RobotControl:
  ```shell
  --add-opens java.desktop/javax.swing=ALL-UNNAMED
  --add-opens java.desktop/javax.swing.plaf.basic=ALL-UNNAMED
  ```
  This was observed on Arch Linux using `java-openjdk17`.

**The application window is just a grey box**

  On systems using a wayland compositor it might be required to define the environment variable `_JAVA_AWT_WM_NONREPARENTING=1`.
  This was observed on Arch Linux using `niri`-WM and `java-openjdk17`.

**Local Java installation**

  If you don't want to install Java system wide you can use a local Java directory by setting the environment variable `JAVA_HOME=<path to your local installation>`.
  You can obtain Java from e.g. [adoptium.net](https://adoptium.net) (Adoptium is from Eclipse Foundation).

### Netbeans
**Netbeans is not found in apt repo when installing on linux**
Netbeans is not included in all `apt` repos. if you have Flatpak installed try:
```
flatpak install netbeans
```

**Netbeans does not find java:**  
![image](../../img/build/netbeans_issue.png)

Solution: Edit the netbeans.conf file. It's located inside the install directory. For example 
`C:/Program Files/NetBeans-12.0/netbeans/etc/netbeans.conf`. Set `netbeans_jdkhome` to the java jdk dir. 
For example: `netbeans_jdkhome="C:\Program Files\Java\jdk1.8.0_271"`
