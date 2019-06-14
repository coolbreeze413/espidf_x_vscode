# espidf_x_vscode
vscode templates to use for ESP-IDF projects.

(disclaimer) inspired by a combination of the following :
- for makefile:
    - https://github.com/pathob/.vscode-esp32
    - https://github.com/VirgiliaBeatrice/esp32-devenv-vscode/blob/master/tutorial.md
- for CMake:
    - https://github.com/Deous/VSC-Guide-for-esp32


Why VSCODE ?

VSCODE is simple, easy to setup, pretty fast, works well, and looks sexy (dark mode please!).

I do like Eclipse as well, alongwith the DevStyle plugin - though I tend toward VSCODE more these days. Also, Eclipse's ability to parse the project, its components, ESP-IDF as well the XTENSA compiler headers does not always work correctly, leading to problems in quick code browsing(which is the main utility of an IDE!)


use:

<dl>
  <dt>vscode-makefile</dt>
  <dd>for ESP-IDF project based on the Makefile build system (default upto ESP-IDF v3.x)</dd>
  <dt>vscode-cmake</dt>
  <dd>for ESP-IDF project based on the CMake build system (default from ESP-IDF v4.0)/dd>
</dl>


Recommended to start using the CMake build system for ESP-IDF projects, because:
- ESP-IDF v3.x already supports CMake build system for all the "component"s.
- Arduino Core (arduino-esp32) as a ESP-IDF component also supports CMake build system.
- It is simple to add support for CMake for existing projects, only the project components need to have a "CMakeLists.txt".


## Steps to get your project to be used with VSCODE:

1. Clone this git repo, and change the following according to your system configuration:
    <table>
      <tr>
        <th>VSCODE DIR</th>
        <th>Variable</th>
        <th>Value</th>
        <th>Comment<th>
      </tr>
        <tr>
        <td>vscode-makefile</td>
        <td>"terminal.integrated.shell.windows"</td>
        <td>path to msys2 bash.exe.<br>The bash.exe on your system can be found at: <PATH_TO_MAKEFILE_TOOLCHAIN>/msys32/usr/bin/bash.exe</td>
        <td>"make" needs to be run on a bash shell.<br>"settings.json" in vscode cannot access environment variables.<br>REF:https://github.com/microsoft/vscode/issues/2809 <br>Maybe it will get fixed one day!</td>
      </tr>
    </table>
    
    For CMake based projects, `vscode-cmake` does not need any changes.
    
    Now, these same vscode templates can be re-used across all ESP-IDF projects on **your** system without any further changes.

2. Ensure that the following environment variables are set correctly :

      <table>
      <tr>
        <th>Variable</th>
        <th>Value</th>
        <th>Comment</th>
      </tr>
      <tr>
        <td>IDF_PATH </td>
        <td>path to the ESP-IDF git repo directory</td>
        <td>If you have projects using arduino-esp32 as a component, then keep a separate ESP-IDF repo checked out to the revision that the arduino-esp32 is based on, and set this path as IDF_PATH when browsing or building these projects.
    For projects based on pure ESP-IDF, remember to set the IDF_PATH to the other ESP-IDF repository on your system, as it will most likely be a pretty recent revision.
    (used in c_cpp_properties.json, used to get the source/include folders for IDF files for IncludePath as well as IntelliSense)</td>
      </tr>
      <tr>
        <td>ESP32_MSYS32_PATH</td>
        <td>path to the ESP-IDF Makefile Toolchain "msys32" directory</td>
        <td>Used in tasks.json to set the command to point to the mingw32.exe as the process to run the build/flash tasks inside.
    Also a convenience environment variable, which is used as basis for fixed definition of XTENSA_ESP32_ELF_PATH below</td>
      </tr>
      <tr>
        <td>XTENSA_ESP32_ELF_PATH</td>
        <td>%ESP32_MSYS32_PATH%\opt\xtensa-esp32-elf</td>
        <td>path to the xtensa-esp32-elf directory inside the ESP-IDF Makefile Toolchain, not needed to change.</td>
      </tr>
      <tr>
        <td>ESP_IDF_TOOLS_PATH</td>
        <td>path to the CMake ESP-IDF-TOOLS install directory</td>
        <td>used in CMake based projects for all operations</td>
      </tr>
      <tr>
        <td>ESP32_COM_PORT</td>
        <td>COM Port used for the current ESP32 board</td>
        <td>used in CMake based projects to upload/flash write etc.</td>
      </tr>
      </table>


3. Ensure that python 2.x is installed, although 3.x is the standard. Always install the python launcher as well, so that launching python3 is default, but python 2.x can be launched using `py -2 <python_file>`

**NOTE** : ESP-IDF CMake Toolchain works correctly with python 2.x only as of now. This will change in the future.

4. Ensure that the following are present in the PATH:
    - Makefile build : `%IDF_PATH%\tools;`
    - CMake build : `%ESP_IDF_TOOLS_PATH%\mconf-idf;%ESP_IDF_TOOLS_PATH%\tools\bin;`

5. copy the appropriate dir (vscode-makefile or vscode-cmake) to the root of the project and rename it to `.vscode`

Open the project folder in VSCODE and enjoy! (takes a few minutes for IntelliSense parsing for a new project)

