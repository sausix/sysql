# Linux Kernel module for a SQL compatible interface to query system information
Still just an idea.

# Why
- It's often hard to write long process pipes with grep, sed, awk, sort and regex for filtering, sorting and formatting data.
- It's often even more harder to read someone elses shell scripts containing some regexes from hell.
- SQL is probably more common and even beginners should be able to read and understand simple queries.
- It would just improve A LOT around scripting under Linux in the future.

# What data?
Most system resources just fit perfectly in a RDBMS result set:
- Processes
- Block devices with size, aliases and filesystem
- Supported filesystems
- System users (/etc/passwd)
- Network interfaces, type, mac address, link speed, ip addresses
- Routes
- Firewall rules
- Kernel config
- Kernel modules
- diskstats
- Memory stats
- systemd-journald
- systemd units and status
- swaps
- vmstat
- mounts

# Why SQL?
Simple, almost human readable. Backward compatible when new columns are introduced in the future. New resources can be added as new tables.  
Joins are perfect to combine the process list to memory usage, owner, the runtime, etc.  
ORDER BY, LIMIT... You get it.


# Why a module?
Linux is basically good enough by supporting "Everything is a file". But these "files" often also need to be filtered, sorted, combined and formatted some how.
Linux today is quite modern and simple. But it doesn't feel like that on any corner. Especially the need of grep, sed, awk and their friends for simple tasks.
It's still basically simple to use these basic building blocks with pipes of course. But they're also an external dependecy to gather system information. And you need to know the path to them...

Not only scripts, but also C and other languages could just access the interface in the procfs or do a system call directly.  
Feels odd if a C program would have to create a userspace program process to get some information.  


## Userspace helper
The communication protocol could match the standard TCP connection or socket stream MariaDB uses. That would allow SQL clients to connect too.  
If there's not a program already to just dump a result set in native format in human readable format, this project could also offer one.


# Why NOT a module
Security. Of course. A kernel module should not introduce critical issues. Don't want to read the project name in the news in a bad context!  
Does it really need the kernel space or does it just waste system resources?  
It would be ok for scripts to spawn a userspace process. Inheriting the UID and just running into permission errors is probably simpler.


# Other related projects
## https://github.com/studentiks/sysql
This project is similar but Python based. I had the same idea for the project name. Isn't it obvious to use that name?

## https://github.com/fmenozzi
Kernel module but only restricted to processes. Does literally use sqlite3 to hold a copy of the processes in memory.

## https://osquery.io/
Multi platform, a lot of resources, but not a kernel module.
