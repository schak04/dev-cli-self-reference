# Linux Commands CheatSheet

## File & Directory Management

- `ls` - List directory contents
- `ls -a` - List all files including hidden ones
- `ls -l` - List files in long format (detailed info)
- `ls -la` - List all files with details
- `cd <dir>` - Change directory
- `cd ..` - Go up one level
- `cd ~` - Go to home directory
- `cd -` - Switch to previous directory
- `pwd` - Print current directory
- `mkdir <dir>` - Create directory
- `rmdir <dir>` - Remove empty directory
- `touch <file>` - Create empty file
- `cp <src> <dest>` - Copy file or directory
- `mv <src> <dest>` - Move or rename file or directory
- `rm <file>` - Delete file
- `rm -r <dir>` - Delete directory recursively
- `rm -rf <dir>` - Force delete directory and contents
- `tree` - Display directory structure as a tree (if installed)

## File Viewing & Text Editing

- `cat <file>` - Display file contents
- `tac <file>` - Display file contents in reverse
- `less <file>` - View file (scrollable)
- `more <file>` - View file (page by page)
- `head <file>` - Show beginning of file
- `tail <file>` - Show end of file
- `echo "text"` - Print text to the terminal
- `echo $VAR` - Print the value of a variable
- `cat > <file>` - Create/overwrite a file with input from the terminal (end with Ctrl+D)
- `cat >> <file>` - Append input from the terminal to a file (end with Ctrl+D)

- `nano <file>` - Edit file in terminal
  - Ctrl+O: Save file
  - Ctrl+X: Exit nano
  - Ctrl+K: Cut line
  - Ctrl+U: Paste line
  - Ctrl+W: Search
  - Ctrl+\: Replace
  - Ctrl+G: Show help
  - Ctrl+C: Show current line/column
  - Ctrl+_ : Go to line number
  - Alt+U: Undo
  - Alt+E: Redo
  - Ctrl+J: Justify (reformat) paragraph
  - Ctrl+T: Spell check (if available)
  - Ctrl+^: Start marking text (select/copy)
  - Ctrl+L: Refresh/redraw the screen

- `vim <file>` - Edit file using Vim editor
  - `i` - Enter insert mode
  - `a` - Append after cursor
  - `A` - Append at end of line
  - `o` - Open new line below
  - `O` - Open new line above
  - `Esc` - Return to normal mode
  - `:w` - Save file
  - `:q` - Quit vim
  - `:wq` or `ZZ` - Save and quit
  - `:q!` - Quit without saving
  - `:x` - Save and quit (like :wq)
  - `:w <file>` - Save as (write to another file)
  - `:e <file>` - Open another file
  - `:r <file>` - Read another file into current buffer
  - `:!<cmd>` - Run a shell command (e.g., :!ls)
  - `:set paste` / `:set nopaste` - Toggle paste mode
  - `:noh` - Remove search highlighting
  - `:qall` - Quit all open files
  - `:wqa` - Save and quit all open files
  - `u` - Undo last change
  - `Ctrl+r` - Redo
  - `dd` - Delete current line
  - `yy` - Copy (yank) current line
  - `p` - Paste after cursor
  - `P` - Paste before cursor
  - `x` - Delete character under cursor
  - `dw` - Delete word
  - `cw` - Change word
  - `/text` - Search for 'text'
  - `n` - Next search result
  - `N` - Previous search result
  - `:%s/old/new/g` - Replace all 'old' with 'new' in file
  - `gg` - Go to first line
  - `G` - Go to last line
  - `0` - Go to start of line
  - `$` - Go to end of line
  - `:set number` - Show line numbers
  - `:set nonumber` - Hide line numbers
  - `:help` - Open Vim help

## Permissions & Executables

- `chmod +x <file>` - Make a file executable
- `chmod 755 <file>` - Set permissions (rwxr-xr-x)
- `chmod 644 <file>` - Set permissions (rw-r--r--, common for text files)
- `chmod 700 <file>` - Owner can read/write/execute, no permissions for others
- `ls -l <file>` - Show permissions and details for a file
- `stat <file>` - Display detailed file status and permissions
- `chown <user> <file>` - Change file owner
- `chown <user>:<group> <file>` - Change file owner and group
- `chgrp <group> <file>` - Change file group ownership
- `umask` - Show default permission mask

**Understanding chmod numbers:**

Each digit in a chmod number represents permissions for user, group, and others, in that order:

- 4 = read (r)
- 2 = write (w)
- 1 = execute (x)

Add the numbers to combine permissions:

- 7 = 4+2+1 = rwx (read, write, execute)
- 6 = 4+2 = rw- (read, write)
- 5 = 4+1 = r-x (read, execute)
- 4 = 4 = r-- (read only)

So, `chmod 755 <file>` means:

- 7 (user): rwx
- 5 (group): r-x
- 5 (others): r-x

And `chmod 644 <file>` means:

- 6 (user): rw-
- 4 (group): r--
- 4 (others): r--

## Superuser & Users

- `who` - Show who is logged in
- `w` - Show who is logged in and what they are doing
- `id` - Show current user and group info
- `sudo <command>` - Run command as superuser
- `su` - Switch to root user
- `passwd` - Change password

## Package Management (Debian/Ubuntu)

- `sudo apt update` - Refresh package index
- `sudo apt upgrade` - Upgrade installed packages
- `sudo apt install <package>` - Install a package
- `sudo apt remove <package>` - Remove a package
- `sudo apt purge <package>` - Remove package including config
- `dpkg -i <package.deb>` - Install .deb manually

## Searching

- `find . -name "*.txt"` - Find all .txt files
- `locate <file>` - Quickly find files by name (if installed)
- `which <command>` - Show path of a command
- `whereis <command>` - Locate binary, source, and man page for a command
- `grep "text" <file>` - Search for text in file
- `grep -r "text" <dir>` - Recursive search in directory

## File Compression

- `tar -cvf archive.tar <dir>` - Create tar archive
- `tar -xvf archive.tar` - Extract tar archive
- `tar -czvf archive.tar.gz <dir>` - Create gzipped tar
- `tar -xzvf archive.tar.gz` - Extract gzipped tar
- `zip archive.zip <file>` - Create zip archive
- `unzip archive.zip` - Extract zip archive

## System Info & Monitoring

- `date` - Show current date and time
- `cal` - Show calendar
- `top` - Live process monitor
- `htop` - Advanced monitor (if installed)
- `free -h` - Show memory usage
- `df -h` - Disk usage by file system
- `du -sh <dir>` - Disk usage of a directory
- `uname -a` - Show system information
- `uptime` - How long system has been up
- `whoami` - Show current user

## Processes & Jobs

- `ps aux` - List all processes
- `kill <PID>` - Terminate a process
- `kill -9 <PID>` - Force kill a process
- `&` - Run a command in background
- `jobs` - Show background jobs
- `fg` - Bring job to foreground
- `bg` - Resume job in background

## Networking

- `ping <host>` - Check network connectivity
- `hostname` - Show or set system hostname
- `ip a` - Show IP address info
- `ifconfig` - Show network interfaces (older systems)
- `curl <url>` - Fetch content from URL
- `wget <url>` - Download file from URL
- `ssh user@host` - Remote login via SSH
- `scp <src> user@host:<dest>` - Copy files over SSH

## Terminal Maintenance

- `clear` - Clear terminal screen
- `reset` - Reset terminal display
- `history` - Show command history
- `!!` - Repeat last command
- `!<n>` - Run command number n from history
- `Ctrl+C` - Cancel current command
- `Ctrl+Z` - Suspend current command

---
