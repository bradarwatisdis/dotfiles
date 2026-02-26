# how the unix kernel works

## startup

** first stage 
 - kernel is loaded by the bootloader

** second stage (system startup)
 - shell started
 - login
 - graphical interface startup
 
_how does the processes work?_

the processes are managed by the os wich stops, shutsdown and starts it. every processes has a "mother" process, as an example, a shell, wich has the init (Ex: busybox) as its mother process.

_what is a shell_
the shell is a "program" responsible for stoping and executing programs, for an example an terminal text editor, as it runs directly on a terminal/shell.

_file system_
the file system is a way to store files in a directory structure. so the user doesnt need to learn the niche parts of the storage system.

_swapping_
swapping is setting a specific part of your storage to swap data between your ram and storage.

