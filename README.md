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


## Sample Project clangstaticanalysis example

This is the starting sample it contains the sourcecode of non self - developed code and self developed code in order to show, how to ignore static code analysis on non self - developed code.

The example demonstrates:
- Basic usage of Cmake for multiple folders
- Using custom clang-format ruleset file
- Using custom clang-tidy configuration file and how to ignore folders