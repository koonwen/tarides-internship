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