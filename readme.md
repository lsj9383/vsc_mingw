# Visual Studio Code(VSC)配置基于MinGW编译器的C/C++说明

## 1.Mingw环境安装

## 2.VSC环境配置

## 3.配置文件
VSC以文件夹的形式管理项目，编译也是按项目为单位进行的，因此需要用VSC打开文件夹。

配置文件一共有四个，均放在`.vscode`文件夹中，包括:
* c_cpp_properties.json
	* 主要用于配置VSC的智能调试。
* tasks.json
	* 主要用于说明如何进行编译得到EXE文件。
* launch.json
	* 主要用于说明如何运行和调试EXE文件。包括配置调试工具路径，传入的参数等等。
* settings.json
	* 显示方式的设置，采用默认即可，甚至可以不用。

在配置中，最需要小心的路径方面的配置，并且路径方面的配置主要分了两个方面:
#### 1).vsc智能提示路径
智能提示路径的作用是使VSC可以找到.h文件的位置，并且可以在输入函数的时候能够对函数所需参数进行提示。gcc编译相关的路径和这个路径没有任何关系。

#### 2).gcc路径
这个是指gcc编译所用到的路径，主要是-I参数指定头文件路径位置，用于在预处理部分可以定位到include的文件并将其展开。

### 1).c_cpp_properties.json
主要是修改Win32下的includePath部分，用于智能提示寻找header。
```json
"name": "Win32",
"includePath": [
	"C:/Program Files (x86)/Windows Kits/8.1/Include/um",
	"C:/Program Files (x86)/Windows Kits/8.1/Include/shared",
	"C:/Program Files (x86)/Windows Kits/8.1/Include/winrt",
	"D:/MinGW/include/",
	"D:/MinGW/lib/gcc/mingw32/6.3.0/include",
	"<other-header-path>"
	"${workspaceRoot}"
],
```

### 2).
```json
{
    "version": "0.1.0",
    "command": "gcc",
    "args": [
        "-g","${file}",
        "-I","inc",			// .h文件搜索路径
        "<other-c-file>",	// 待链接的文件以及库
        "-o","${fileDirname}/${fileBasenameNoExtension}.exe"],
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": ["relative", "${workspaceRoot}"],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}
```

### 3).lauch.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceRoot}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\MinGW\\bin\\gdb.exe",
            "preLaunchTask": "gcc",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

### 4).settings.json
该文件过长，并可以忽略，因此不给出具体代码。需要settings.json源码可以在[这里](gccdemo/.vscode/settings.json)查看。