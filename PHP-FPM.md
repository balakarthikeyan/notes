# PHP-FPM Key Features
PHP-FPM includes numerous features that can prove beneficial for websites receiving traffic in large volumes frequently. 

- Ability to start workers using various uid/gid/chroot/environment and php.ini, which replaces the safe mode users may expect
- In-depth management for simple stop/start processing
- Logging of stdout and stderr
- Emergency restart available, in the event of an opcode cache being destroyed accidentally
- Support for uploads is faster
- Based on php.ini configuration files
- Slowlog variable configuration for detecting functions that take longer than usual to execute
- FastCGI improvements, with a special function for stopping and downloading data while completing long processes (e.g. processing statistics)
- Basic stats are available, similar to the mod-status module in Apache

PHP-FPM can use one of three process management types: static, dynamic, ondemand

## Static
Static ensures a fixed number of child processes are always available to handle user requests.
In this mode requests don’t need to wait for new processes to startup, which makes it the fastest approach.

you would configure it in /etc/php/7.2/fpm/pool.d/www.conf 
```
pm = static 
pm.max_children = 10 
```
## Dynamic
In this mode, PHP-FPM dynamically manages the number of available child processes and ensures that at least one child process is always available.

This configuration uses five configuration options; these are:

pm.max_children: The maximum number of child processes allowed to be spawned.
pm.start_servers: The number of child processes to start when PHP-FPM starts.
pm.min_spare_servers: The minimum number of idle child processes PHP-FPM will create. More are created if fewer than this number are available.
pm.max_spare_servers: The maximum number of idle child processes PHP-FPM will create. If there are more child processes available than this value, then some will be killed off.
pm.process_idle_timeout: The idle time, in seconds, after which a child process will be killed.

Setting	Value
max_children	    (Total RAM – Memory used for Linux, DB, etc.) / process size
start_servers	    Number of CPU cores x 4
min_spare_servers	Number of CPU cores x 2
max_spare_servers	Same as start_servers
process_idle_timeout which is the number of seconds after which an idle process will be killed.

vagrant@vagrant:~$ grep MemTotal /proc/meminfo
MemTotal:        2095136 kB

vagrant@vagrant:~$ free -hl
              total        used        free      shared  buff/cache   available
Mem:           2.0G         96M        1.5G         34M        408M        1.7G
Low:           2.0G        504M        1.5G
High:            0B          0B          0B
Swap:          979M          0B        979M

banandakumar@eqs-hk-wordpress-wps001:~$ grep MemTotal /proc/meminfo
MemTotal:        2001920 kB

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
pm.max_requests = 500