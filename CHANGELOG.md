1.4.21
======
* Fixed Debug Adapter startup issue

1.4.19
======
* Execute default CABL rules when no license configured in VS Code settings

1.4.17
======

* IMPORTANT: the .builder/storage directory of your projects has to be deleted after upgrade. Content will be automatically regenerated.
* New assembly catalog command ; improved .Net code completion
* Various code completion / hover / definition improvements in LS
* MaxThreads implementation (#58)
* Javadoc & deprecated methods (#97)

1.4.15
======

* All class symbols now available in SonarLint ; result of the analysis should now be on par with Sonar Scanner
  * Code will be backported to SonarLint Eclipse (if possible)
* Skip rcode scan of procedures during project startup (scan only classes)
* Fix mapping of include file names to parse units (issue #91)

1.4.13
======

* SonarLint integration in Language Server (standalone mode only, connected mode available soon)
  * Change required in `.vscode/cabl.json`, see README.md
* Improved code completion

1.4.11 & 1.4.12
===============

* Upgrade CABL rules to 2.21.1 (note that if you have a valid CABL subscription, you can execute the CABL rules in VSCode, open the license management website to get your key)
* OpenEdge 12.7 support in configuration files

1.4.10
======

* Allow INI file to be overriden when running procedures with prowin

1.4.9
=====

* Fix deactivate entry point (stop LS locally and on remote workspaces)
* Code completion improvements
* Report errors on duplicate project names
* Report errors in VSCode UI when OE sessions can't be started

1.4.8
=====

* Proenv entry in Terminal view
* Code completion improvements

1.4.7
=====

* Syntax highlight (minor improvements)
* Code outline view improvements

1.4.6
=====

* New status bar, and faster feedback when loading large projects
* Fix issue when setting breakpoints (which were never set correctly under specific circumstances)

1.4.5
=====

* Fix major regression in all commands (nothing executed + Java stack trace displayed in LS output)

1.4.4
=====

* Alt+L shortcut: show source code of debug listing line
* Code completion on static methods (work in progress)
* Improved status bar

1.4.3
=====

* JVM extra arguments can now be configured in settings

1.4.2
=====

* Build output directory can now be an absolute directory

1.4.0
=====

* Add `documentation` attribute in `buildPath` entries (use JsonDocumentation task from PCT to generate those files)
  * Deprecated methods will be highlighted in source code
* New actions: generate XREF and XML-XREF
* New option to hide/display objects from include files in outline view
* Various code completion improvements
* Multiple minor bugfixes (don't hesitate to report them!)

1.3.11
======

* Support for 32 bits Progress install
* Fixed Debug Adapter startup
* Fixed session startup issues when extraParameters attribute wasn't found
* Fixed InvalidRCodeException when parsing lots of rcode
* Removed error message when parsing windows-* DF file footer
* Upgraded bundled CABL rules

1.3.10
======

* CABL rules upgrade (now match parser version number)
* Code completion improvements on classes

1.3.9
=====

* Fixed major performance regression in 1.3.8
* Fixed handling of code completion of THIS-OBJECT and SUPER

1.3.8
=====

* New actions: preprocess code + generate debug listing
* Monitor openedge-project.json for changes (DB connection, propath, ...)
* New "Build mode" setting: compile files (or not) depending on the value
* BETA: New "documentation" attribute in propath entries: use [PCT JsonDocumentation task](https://wiki.rssw.eu/pct/JsonDocumentation.md)
* Improved code completion engine
* Declaration of class variables automatically add a 'USING' statement

1.3.x
=====
- Extension overhaul, lots of functionalities moved to ABL Language Server

1.1
=====
- Code completion and Outline pane

1.0
=====
- You can now specify a startup procedure

0.9
=====
- New definition provider: the outline pane is now filled with the definitions found in the current document

0.8
=====
- You can now define the dlc value from the config file (optional)

0.5
=====

## What's new
- `proPath` and `proPathMode` supports in `.openedge.json` config file

0.4.2
=====

## What's new
- Better syntax highlighting (define stream scope)

0.4.1
=====

## Bug fixes
- full primitive type (ex: character) matches only abbrev (ex: char)

0.4.0
=====

## What's new
- Better syntax highlighting (parameter vs variable)
