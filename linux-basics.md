# Linux Basics

## File System Navigation
- `pwd` - Print working directory
- `ls [options] [directory]` - List directory contents
  - `-l` Long format
  - `-a` Show hidden files
  - `-h` Human-readable sizes
- `cd [directory]` - Change directory
  - `cd ~` or `cd` - Go to home directory
  - `cd -` - Go to previous directory

## File Operations
- `cp [options] source destination` - Copy files/directories
  - `-r` Copy directories recursively
  - `-v` Verbose output
- `mv [options] source destination` - Move/rename files
- `rm [options] file` - Remove files/directories
  - `-r` Remove directories and their contents
  - `-f` Force removal

## Text Processing
- `cat [file]` - Display file contents
- `less [file]` - View file with pagination
- `head/tail [options] [file]` - Display beginning/end of file
  - `-n [number]` Show specified number of lines
- `grep [options] pattern [file]` - Search text patterns
  - `-i` Case-insensitive search
  - `-r` Recursive search

## System Information
- `uname -a` - Show system information
- `df -h` - Show disk space usage
- `free -h` - Show memory usage
- `uptime` - Show system uptime and load average

## Process Management
- `ps aux` - List all running processes
- `top` / `htop` - Interactive process viewer
- `kill [signal] PID` - Send signal to process
  - `kill -9 PID` - Force kill process
- `bg` / `fg` - Control background/foreground processes

## Help and Documentation
- `man [command]` - Show manual page
- `[command] --help` - Show command help
- `whatis [command]` - Brief command description
- `which [command]` - Show full path of command
