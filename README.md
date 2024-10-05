# Sample projects for usage of sw quality tools for C from within Visual Studio Code, i.e.

- Build System: CMake
- Build System: Ceedling
- Static Analysis: clang-format (coding guidelines)
- Static Analysis: clang-tidy (clang tidy rule management)
- Unit Testing: Unity, Tests called with CTest
- Mocking: FFF (with CMake only)
- Mocking: Cmock (with Ceedling only)

Generating reports via CMake calls include:
- clang-format call for whole solution
- clang-tidy call for whole solution and generating report
- gcov and lcov call and generating coverage report

## Basic Tool Setup
The samples assume, latest version of VS Code is used with following plugins:
- C/C++
- CMake Tools
- CMake

In order to use Clang and its basic way of configuration, VSCode and C/C++ Plugin require some settings to be made:

- editor.formatOnSave
- editor.formatOnType - after typing ";" formating happens
- c_cpp.clang_format_style must be set on "file"

*Note: The C/C++ plugin provides multiple settings for clang. In case of problems with clang, check the settings (go to file -> settings, in search bar search for "clang"). Rulesets, compiler options etc. can be configured via settings here, what we generally don't want to do.*

## Sample Project clangstaticanalysis-example

This is the starting sample it contains the sourcecode of non self - developed code and self developed code in order to demonstrate, how to ignore static code analysis on non self - developed code. The sample explicitely contains violations.

The example demonstrates:
- Basic usage of Cmake for building artifacts in multiple folders
- Using custom clang-format ruleset file
- Using custom clang-tidy configuration file and how to ignore folders

*Note: In other demos, we will use a CMAKE target to call clang-tidy via commandline, outside of visual studio. The results from visual studio integration and external command line call are different! This may be due to different internal default settings in Visual Studio Code. Therefore, for formal releases, use the command line based call, as these results will be repeatable in other environments, e.g. CI environment*

## Sample Project fff-unity-cmake-example

This sample builds on the code of the initial sample "clangstaticanalysis-example" and adds unit tests of the production code with unity framework and the fff mocking framework. The sourcecode from Unity and fff have been downloaded and directly stored in project directories.

The example demonstrates:
- How to build the unittests with Cmake, resolving the dependencies to unity and fff
- How to run the unit tests directly from within Visual Studio Code by including CTest

## Sample Project fff-unity-cmake-gcov-example

This sample adds nothing to the previous production or test code, but adds output generation for reports:
- Test Coverage with gcov
- Static analysis with calling clang-tidy/clang-format from command line, storing the output file and generating a basic human readbale report with the clang_html python module.

*Note: The sample only works under linux and has been run under Ubuntu 22.04. As stated in the clangstaticanalysis-example: The analysis contains different warnings as when directly executed from Visual Studio Code. Therefore, in real projects, use the "outside IDE approach" for formal static analysis testing.