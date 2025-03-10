# Changelog for Linux-Installer-Script

## 2021-08-05
* Expand Docker detection in setup to make it work during Docker build process in GitHub Actions 

## 2021-06-18
* Remove info adapter from new installations by default

## 2021-04-07
* Install backitup Adapter on new installations by default

## 2021-01-23
* (Linux) fix CLI completions
* (Linux) revert "ignore which aliases"

## 2021-01-20
* (Linux) enable auto-completion for iobroker commands
* (Linux) ignore which aliases
* (Linux) updated native packages so canvas can be built by default
* (macOS) disabled file permission check on OSX
* fixed cirrus tests
* added support of npm7

## 2020-12-07
* (FreeBSD) Fixed installation for FreeBSD by using `/usr/bin/env` to detect path for `bash` automatically and do not enforce usage of python 2.7 which is EOL
* (Linux) Use `-y` argument for `yum`

## 2020-06-19 
* (Linux) Updated Recommended Node.JS and NPM Version to 12.x LTS and improved CheckVersions Output for the user

## 2020-06-12
* (Linux) Added net-tools to fix error #277 "ifconfig: command not found" 
* (Linux) correctly parse string arguments inside quotes

## 2020-04-12
* (Linux) Avoid entering the sudo password for iobroker CLI

## 2020-01-30
* (Linux) Add iobroker user to the `video` group

## 2020-01-25
* The installer lib file is now deleted after sourcing it
* Configure `npm` to enforce engine versions in `package.json`

## 2020-01-13
* The `shutdown` command is no longer limited to `-h now`
* The following services are now started before ioBroker if possible:
    * Influx DB
    * MySQL Server
    * Maria DB

## 2019-11-29
* Add user to `video` group (Linux)

## 2019-11-26
* (ADOE) Extracted many shared Installer/Fixer functions into a common library script

## 2019-11-03
* FreeBSD should now finally be supported correctly

## 2019-10-21
* (ADOE) Large refactoring:
    * refactored 3 repeated execution blocks into function "add2sudoers()"
    * introduced var $SUDOX as shortcut for "if $IS_ROOT... then ... else ... fi"
    * refactored detection of HOST_PLATFORM into function get_platform_params()
    * extended function "get_platform_params()": now delivers vars: HOST_PLATFORM, INSTALL_CMD, IOB_DIR, IOB_USER
    * changed "brew" and "pkg" to "$INSTALL_CMD"
    * refactored "Enable colored output" into function "enable_colored_output()"
    * "Install Node.js" and "Check if npm is installed" were existing twice. Deleted one.
    * refactored "Determine the platform..." to function  "install_necessary_packages()"
    * calling "install_package()" instead of "install_package_*"
    * refactored "Detect IP address" tu function "detect_ip_address()"
* Added option to choose another npm registry.  
Use `MIRROR=taobao curl -sL https://iobroker.net/install.sh | bash -` to install ioBroker using the taobao registry

## 2019-10-19
* Install `python-dev` to fix npm error: `ImportError: No module named compiler.ast`

## 2019-10-18
* Emergency fix to last change: escape `$` in `$(pwd)`

## 2019-10-13
* Always run `npm` as iobroker when inside installation dir

## 2019-09-30
* Allow passwordless sudo for `mysqldump`
* Allow passwordless sudo for `ldconfig`

## 2019-09-25
* Disable any warnings related to `npm audit fix`

## 2019-09-16
* Install Node.js if it is not installed
* Support of CentOS and AWS AMI

## 2019-07-21
* suppress warnings during npm install

## 2019-07-17
* Fix for Debian 10: Add `/sbin` and similar directories to `PATH` at the start of the script

## 2019-07-03
* Include `PATH` environment variable in OSX startup script

## 2019-06-29
* Add install fixer as iobroker shortcut via "iobroker fix"
* Autodetect `bash` path to fix `command not found` on FreeBSD

## 2019-05-14
* (Linux) Add iobroker user to the `audio` group

## 2019-04-04
* Docker: Auto-detect if the container has `CAP_NET_ADMIN` and give it to `node` if possible

## 2019-03-15
* Add `-H` flag to `sudo -u iobroker` to fix EACCES errors when using the `iobroker` commands to install stuff.

## 2019-03-10
* Don't set CAP_NET_ADMIN in Docker
* Fixed the group add command in FreeBSD

## 2019-03-06
* Fixed the setcap command so it works in Docker
* Fixed another typo in FreeBSD installation routine

## 2019-03-05
* Fixed typo in FreeBSD installation routine

## 2019-03-04
* Also set the correct ACLs when running the script as root

## 2019-03-03
* Allow the commands needed by RPI2

## 2019-03-01
* Add redis as a dependency to the `systemd` unit and `init.d` script to avoid deadlocks on shutdown
* Removed limitation for number of arguments for iobroker

## 2019-02-25
* fix setcap and include all in one command

## 2019-02-23
* Give nodejs access to raw devices like ble

## 2019-02-15
* (Linux) Add iobroker user to the redis group

## 2019-02-03
* (Linux) Add iobroker user to the i2c group

## 2019-01-30
* (Linux) Give NodeJS access to privileged ports (<1024 and Bluetooth)
* (MacOS) Add package installing support (brew) and autostart support for 

## 2019-01-25
* (FreeBSD) Added added a procedure to handle the freebsd package installation (there is no apt on BSD). `install_package_freebsd()`
* (FreeBSD) Added a rough list of packages for iobroker to run on FreeBSD (subject to further improvement).
* (FreeBSD) Added config patches for the zero conf daemon processes, add them to rc startup and start them.

## 2019-01-23
* Revert the `KillMode` change
* Redirect `iobroker {start,stop,restart} adaptername` to `node` when using `systemd`.  
**Note:** If you cannot start/stop adapters using the command line, you have to edit the iobroker binary:
  ```
  sudo nano $(which iob)
  ```
  and change
  ```
  if [ "$1" = "start" ] || [ "$1" = "stop" ] || [ "$1" = "restart" ]; then
  ```
  to
  ```
  if (( $# == 1 )) && ([ "$1" = "start" ] || [ "$1" = "stop" ] || [ "$1" = "restart" ]); then
  ```
  Then exit and save.


## 2019-01-22
* Use `KillMode=process` in `systemd` to prevent detached processes from being killed aswell

## 2019-01-21 (fixes #106, #107)
* Move temp_sudo_file instead of copying
* Add current user to iobroker group

## 2019-01-20 (see #99)
* User creation and specifying which commands may be executed as sudo without password
* Creation of the startup files for /etc/init.d, systemd (Linux/OSX) and rc.d (FreeBSD), including detection of the node executable on startup
* Creation of the executables iob and iobroker
* Automated installation of commonly used packages
* More logs into INSTALLER_INFO.txt
* Automatic IP address detection for the final message
* Detection if the installer script is being run as a result of npm install or some other command. _This should fix failures during execution of npm rebuild._
* Run all iobroker commands as the iobroker user if possible
* Fix iobroker start/stop/restart/status when systemd is used

**Note:** Since so much is now being done in the installer script, manual installations on Linux/OSX/FreeBSD using `npm` are forbidden since `v2.0.0`, which was released on 2019-01-21.


## 2019-01-02 (and earlier)
* this version introducted writing INSTALLER_INFO.txt into the ioBroker directory with installation details used later on for support reasons
* initial versions of the script and added several stuff, too much to describe here. This is used as baseline for the shellscript

