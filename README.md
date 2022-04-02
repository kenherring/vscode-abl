# OpenEdge ABL support for Visual Studio Code
This extension provides rich OpenEdge ABL support for Visual Studio Code.

## Current Status
This extension is based on the existing Christophe Camicas work, but going through a complete overhaul due to the addition of the Language Server. Despite its version number, the extension is currently considered alpha or beta quality software.

## Features
* Syntax highlight
* OpenEdge project configuration (propath, database connections, aliases, ...)
* Multi-thread background compiler
* Run profiles (currently not active)
* Debugger (currently not active)
* ABLUnit Support (currently not active)
* Code completion (work in progress)

## Global Configuration
OpenEdge runtimes have to be declared in VSCode configuration file. Open settings `(Ctrl + comma)` -> Extensions -> ABL Configuration -> Runtimes, or modify `settings.json`:
![Settings](resources/images/settings.png)

## Project Configuration
OpenEdge projects can be configured in a file called `openedge-project.json`. This file has to be in the root directory of the project.
```json
{
  "name": "MyProject", # Project name, will be used in the future for dependency management
  "version": "1.0",    # Project version number, will be used in the future for dependency management
  "oeversion": "12.2", # Must reference an existing ABL version in Settings -> Extensions -> ABL Configuration -> Runtimes
  "graphicalMode": true, # True for prowin[32], false for _progres
  "charset": "utf-8", # Charset 
  "extraParameters": "", # Extra Progress command line parameters
  "buildPath": [
    # Entries can have type 'source' or 'propath'. Path attribute is mandatory. Build attribute is optional (defaults to 'path'). Xref attribute is optional (defaults to 'build/.pct' or '.builder/srcX')
    { "type": "source", "path": "src/procedures" },
    { "type": "source", "path": "src/classes" },
    { "type": "source", "path": "src/dev", "includes": "foo/**,bar/**", "excludes": "foo/something/**" }
    { "type": "propath", "path": "${DLC}/tty/netlib/OpenEdge.net.pl" }
  ],
  "buildDirectory": "build", # Optional global build directory. 
  "dbConnections": ["-db db/sp2k -RO"], # One entry per database
  "dumpFiles": ["dump/sp2k.df"], # Required by the parser and lint rules
  "aliases": "sp2k,foo,bar", # Required by the parser and lint rules
  "numThreads": 1, # Number of OpenEdge sessions handling build
  "procedures": [ # List of procedures, started before the main entry point (and after DB connection and propath configuration)
    { "name": "foo/bar.p", "mode": "once" /* Mode can be once, persistent or super */ }
  ],
  "profiles": [ /* See section below */ ]
}
```

## Activation
The extension is activated when a `.p`, `.w` or `.cls` file is opened. ABL actions may fail before the extension is activated.

## Actions & Keyboard Shortcuts
The following actions are defined in this extension (use Ctrl + Shift + P to execute actions):
* Restart ABL Language Server: restart the background Java process (and OpenEdge sessions)
* Rebuild project: delete all rcode and recompile all files
* Open File in AppBuilder: start the AppBuilder (with DB connections and propath) and open current file
* Open Data Dictionary
* Run with Prowin: execute current file in prowin[32] session
* Run with _progres: execute current file in _progres session (with `-b`)
* Switch to profile: switch current project to another profile

The following keyboard shortcuts are configured by default:
* F2: Run current file (in batch mode with _progres)
* Shift + F8: Open current file in AppBuilder

## Extra Profiles
On top of the default profile configured in `openedge-project.json`, additional profiles can be configured in the `profiles` section. Each profile is defined by a name, parent's name (optional) and a set of values. For example:
```json
{
  /* Default profile values */
  "version": "12.2", "graphicalMode": false, "dbConnections": "-db db/sp2kv12 -RO",
  "profiles": [
    { "name": "V11", "inherits": "default", "value": { "oeversion": "11.7", "dbConnections": "-db db/sp2kv11 -RO" } },
    { "name": "V12.5 GUI", "value": { "oeversion": "12.5", "graphicalMode": true }}
  ]
}
```
V11 Profile inherits from the default profile, so graphicalMode will be set to true. OpenEdge version and DB connections are specified in the profile.
V12.5GUI Profile doesn't inherit from the default profile, so it won't have any DB connection.
When opening a project, VSCode will check for `.vscode/profile.json`. If this file is present, then this profile will be loaded. Otherwise, the default profile will be used. It is recommended to add this file to the SCM ignore list.

## Debugger

** Debugger is currently inactive **

You can use the debugger to connect to a remote running process (assuming it is debug-ready), or run locally with debugger.

You first need to create the launch configuration in your `launch.json` file, 2 templates are available, one for launch and the other for attach).

```JSON
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach to process",
            "type": "abl",
            "request": "attach",
            "address": "192.168.1.100",
            "port": 3099
        }
    ]
}
```

To attach to a remote process, it needs to be [debug-ready](https://documentation.progress.com/output/ua/OpenEdge_latest/index.html#page/asaps/attaching-the-debugger-to-an-appserver-session.html).
The easiest way to achieve that is to add `-debugReady 3099` to the startup parameters (`.pf` file) of your application server.

The debugger supports basic features
- step-over, step-into, step-out, continue, suspend
- breakpoints
- display stack
- display variables
- watch/evaluate basic expressions

You can map remote path to local path (1 to 1) using `localRoot` and `remoteRoot`. This is useful when debugging a remote target, even more if it only executes r-code.
`localRoot` is usually your `${workspaceRoot}` (current directory opened in VSCode). `remoteRoot` may remains empty (or missing), in this particular case, the remote path is relative, and resolved via the `PROPATH` by the remote.


You can also map different remote path to local path via source mapping `sourceMap`. This is useful if you don't have all the source code in a unique project (ex dependencies).

## Unit tests

** Unit tests are currently inactive **

Based upon the ABLUnit framework (need to be installed locally), you can specify launch parameters to find and execute test files
```
{
    "test": {
        "files":[
            "tests/*.test.p"
        ],
        "beforeEach": {
            "cmd": "%ProgramFiles%\\Git\\bin\\sh.exe",
            "args": [
                "-c",
                "echo starting"
            ]
        },
        "afterEach": {
            "cmd": "%ProgramFiles%\\Git\\bin\\sh.exe",
            "args": [
                "-c",
                "echo done"
            ]
        }
    }
}
```

## Greetings
Initial plugin development done by [chriscamicas](https://github.com/chriscamicas). In turn, largely inspired by ZaphyrVonGenevese work (https://github.com/ZaphyrVonGenevese/vscode-abl).
Also inspired by vscode-go and vscode-rust extensions.

Thanks to all the contributors: mscheblein

## License
VSCode Plugin Code is licensed under the [MIT](LICENSE) License.
Language Server code is (c) Copyright Riverside Software.
