# VS Code GCC C++ Configuration

## Introduction

This is a personal project that includes the configuration files necessary to get up and running in VS Code using C/C++.

CMake is not used, so this is not a go-to solution for large projects.

Note: **C++ is the focus for this config**, so unexpected results may occur for compilation of C.

### Prerequisites

Follow this guide up until the _Create Hello World_ section:

- https://code.visualstudio.com/docs/cpp/config-mingw#_cc-configurations
- Notes:
  - I had to put the `/bin` path in _System Variables_ before I could use the `gcc` command outside of the MSYS2 console
  - This project assumes you have used the default `C:\msys64` installation path. You will need to edit the configuration if this is not the MSYS2 installation path

### Cool Resources

- Online C++ compiler that shows assembly output with extensive configuration: https://godbolt.org/
- A great resource to help with modern practices: https://lefticus.gitbooks.io/cpp-best-practices/content/

## Installation

1. Copy and paste the files in `/.vscode` to your local project
2. **Keep reading** so you know what is going on

## Explanation & Editing Guide

### launch.json

This file handles debugging.
Most of this was generated by VS Code using one of their default launch options for GCC.

#### The field you need to pay attention to:

- `"program"`
  - This is the path to the executable generated by the compiler
  - This relative path assumes that the file you have open is the main file **AND** the executable is named after the file
    - Example: You have `main.cpp` open, so the debugger assumes the executable is `main.exe`
  - Update this so it points to the executable you wish to debug
  - **NOTE: Make sure you have compiled your code without the optimization flags (`-O3`, `-O2`, ...) and you have included the `-g` flag, otherwise you will have one heck of a headache trying to figure out why the debugger is acting weird**

### tasks.json

This file creates two tasks that build the project.

- To run the default task, which generates a debuggable exe: `Ctrl+Shift+B`
- To select the other one: `Terminal > Run Task...`

#### The fields you need to pay attention to:

- `"label"`
  - Make sure you know which task you are selecting
- `"args"`
  - **VERY IMPORTANT**
  - Contains compiler flags taken from _cpp-best-practices_ linked above
  - The difference between the Production and Debuggable tasks is the `-O3` and `-g` flags
    - Removing these flags optimizes the code and disallows debugging
  - Make note of the `"${workspaceFolder}\\*.cpp",` line
    - This will compile ALL .cpp files in the project
    - To include subdirectories, add `"${workspaceFolder}\\some_directory\\*.cpp",` after this line
