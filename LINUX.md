# Linux Sysadmin Tricks
Just a log of command line tricks

## User setup
Setting up and configuring a user account on Mirage server
- useradd
- userpwd
- usermod
- command-line configs taken from mirage user .bashrc file
- adding ~/.ssh/authorized_keys

## Installing from source
Download tarball -> Check legitimacy -> Unzip -> Configure -> Build -> Install
- `$ wget < link >` is a useful command for downloading files from source
- Once tarball is downloaded, check that the md5sum corresponds to what is written at source by using `$ md5sum < tar file >`
- uncompress using `$tar -xfv < tar file >`
- Go into the unzipped directory and configure. `./configure` scripts are important to specify the build directory/compiler and other settings for the Makefile
 - Sometimes `make` fails because some header files can't be found. Usually if header files are missing even though you have the package, try downloading the `<package name>-devel` package which should have the headers.

## Reading object files
To see what is inside an object file
- `readelf` is a tool for reading objet files
- using the `-s` flag, we can get readelf to show us the symbols (names) in the file
- We can notice that some symbol names of functions show up as "UND" which means that they are undefined. This is because we have declared their declaration and not its definition.

## Bash
Bash tricks
- `$(command)` and `'command'` (backticks) pre-evaluate the bash command such as `pwd` for example

# Git
How to manage Git repos and have better Git workflows

## **TODO**
graph out types of useful workflows

## Rebasing
- Rebasing means that git will stack your commits on top of the previous commits in the remote and you will need to deconflict each of your own commits one by one before pushing to the main repo

