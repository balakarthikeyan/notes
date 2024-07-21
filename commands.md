# Linux Commands
```
`ll`	to list the files
`tree <foldername>`	to list files in tree structure
`apt-get`	is command line software for installing packages on Ubuntu
`history | tail 10`	to show only 10 history log
`history -c` clear history
`echo "$HISTFILE"`	to echo path of history
`rm ~/.bash_history`	remove history file
`echo 'history -c' >> ~/.bash_logout`	to delete history while logout
`dpkg -l | grep php | tee packages.txt`	check the content and write in packages.txt
`ls`	Show directory contents (list the names of files).
`cd`	Change Directory.
`mkdir`	Create a new folder (directory).
`touch`	Create a new file.
`rm`	Remove a file.
`cat`	Show contents of a file.
`pwd`	Show current directory (full path to where you are right now).
`cp`	Copy file/folder.
`mv`	Move file/folder.
`grep`	Search for a specific phrase in file/lines.
`find`	Search files and directories.
`vi/nano`	Text editors.
`history`	Show last 50 used commands.
`clear`	Clear the terminal screen.
`tar`	Create & Unpack compressed archives.
`wget`	Download files from the internet.
`du`	Get file size.
```
ps -o pid,user,%mem,command ax | sort -b -k3 -r
sudo pmap 917 | tail -n 1 | awk '/[0-9]K/{print $2}'

ps aux | head -1; ps aux | sort -rnk 4 | head -5
alias mem-by-proc="ps aux | head -1; ps aux | sort -rnk 4"

lscpu | egrep 'Model name|Socket|Thread|NUMA|CPU\(s\)'
```
CPU Core: 8
Thread per core: 2
Total threads: 16 ( CPU core[8] * Thread per core [2])
```
echo "CPU threads: $(grep -c processor /proc/cpuinfo)"
echo "Threads/core: $(nproc --all)"

lscpu -b -p=Core,Socket | grep -v '^#' | sort -u | wc -l

netstat -tulpn
pgrep -fl <name>
adb logcat | grep -i "nameofyourapp"