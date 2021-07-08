GENERAL

history, e.g. history | grep 'cd'

dpkg -l

grep -ir 'string' ./ (search ‘string’ in files of local folder)

adduser aegir www-data    (make aegir a user of group www-data)

su - teo OR su -s /bin/bash - aegir (change user)

sudo passwd tkosmop (reset password for user `tkosmop`)

find / -type l 2>&1 | grep drush (search for symbolic links containing drush)

lsb_release -a (get linux distribution)

cat /etc/*-release (get linux distribution)

uname -m (get system architect 32bit or 64bit)

who (check which users are logged in)

hostname (prints out computer name)

keytool -list -keystore sibex_push.keystore (list keystore info)

tar -czvf name-of-archive.tar.gz /path/to/directory-or-file (Compress an Entire Directory or a Single File)

tar -xzvf archive.tar.gz (Extract an archive)

unzip archive.zip (unzip zip file)

watch -n2 “ls -al” (repeat command “ls -al” every 2 secs)

Ctrl+r and type string (search for string and use Ctrl+r to enumerate through search results)

find . -empty -type f (list empty files in current directory)

wc -l (count lines) e.g. to count empty files in current directory: find . -empty -type f | wc -l




NETWORK

netstat -natp, active internet connections
http://www.computerhope.com/unix/unetstat.htm

telnet

nmap, e.g. nmap 192.168.1.69 (scan ip for listening ports)

arp-scan -l --interface=eth0 (check connected computers in local network)

Ifconfig (equivalent to windows ipconfig /all) OR /sbin/ifconfig

scp -i latramis.pem /var/www/json.php ubuntu@54.229.54.53:/var/www/html/test 
(copy local file json.php to remote server through ssh with public key)

scp -i latramis.pem ubuntu@54.229.54.53:/var/www/html/info.php /home/teo/Desktop/ (copy remote file info.php to local system through ssh with public key)

nc (netcat), nc -v www.inbroker.com 443
Send command (e.g. reset) to inbroker java servers (2 bytes[most,least significant] must precede command):
echo -n -e \\00\\05reset > reset.jcmd or echo -n -e '\0000\x16resetMarket VINE false' > reset.jcmd
nc 193.242.251.39 3040 < reset.jcmd


SYSTEM
systemd-analyze, https://www.freedesktop.org/software/systemd/man/systemd-analyze.html
blkid,
ps aux | less (list all processes)
http://www.cyberciti.biz/faq/show-all-running-processes-in-linux/
kill process_id (kill process gracefully)
kill -9 process_id (kill process)
Hold super key to display system keyboard shortcuts
/sbin/chkconfig --list, display all services configured to start on system boot, display is per run level
cat /etc/inittab, display available run levels
