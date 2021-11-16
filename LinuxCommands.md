# Linux commands <!-- omit in toc -->


- [google chrome](#google-chrome)
- [grep](#grep)
- [find](#find)
- [lsblk](#lsblk)
- [fsck](#fsck)
- [df](#df)
- [top](#top)
- [dmesg](#dmesg)
- [lsof](#lsof)
- [mount/umount](#mountumount)
- [administer linux users](#administer-linux-users)
- [less](#less)
- [curl](#curl)
- [wget](#wget)
- [vi editor](#vi-editor)
- [ln](#ln)
- [pkill](#pkill)
- [which](#which)
- [scp](#scp)
- [cp](#cp)
- [zip](#zip)
- [cut](#cut)
- [sort](#sort)
- [uniq](#uniq)
- [awk](#awk)
- [id](#id)

## google chrome

start with specific profile
```
google-chrome --profile-directory=Default
```

check all profiles
```
cat ~/.config/google-chrome/Local\ State | jq .profile
```


## grep

[Reference](https://www.computerhope.com/unix/ugrep.htm)

search for string WINBANK in file ext.txt.backup and display first 70 chars of each line in which string is found
```
grep "WINBANK" ext.txt.backup | grep -oE "^.{70}"
```

search for string WINBANK in file ext.txt.backup and return only first line found
```
grep -m 1 WINBANK ext.txt.backup
```

display number of lines string WINBANK is found in file ext.txt.backup
```
grep -c "WINBANK" ext.txt.backup
```

display number of lines regexp is found in file ext.txt.backup
```
grep -cE "^.*(NYSE).*WINBANK.*$" ext.txt.backup
```

search for regexp and display pattern "company=...userName=..." from each line in which regexp is found, with no repeated lines (grouped) along with number of each grouped line (sort | uniq -c) 
```
grep -E "^.*(NYSE).*$" ext.txt.backup | grep -oE "company=\w*\,.*userName=\w*" | sort | uniq -c
```

search for regexp and display pattern "company=..." from each line in which regexp is found, with no repeated lines (grouped) along with number of each grouped line (sort | uniq -c) 
```
grep -E "^.*(NYSE).*$" ext.txt.backup | grep -oE "company=\w*" | sort | uniq -c
OR
grep -E "^.*(company).*$" ext.txt.backup_FC1 | grep -oE "company=\w*" | sort | uniq -c
```

search non-matching regexp (invert match -v)
```
grep -vE "^.*(Ping|Status).*$" ext.txt.backup_FC1_EUR
```

search string `git` in folder `.git`, [Reference](https://stackoverflow.com/a/16957078)
```
grep -rnw '.git' -e 'git'
```

## find

search for `ts` files in current folder with exclusion of folder `./node_modules`, [Reference](https://stackoverflow.com/a/15736463)
```
find . -name '*.ts' -not -path "./node_modules/*"
```

## lsblk

[Reference](https://www.linuxnix.com/lsblk-command-explained-with-examples)

list block devices
`sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL`

## fsck

[Reference](https://www.tecmint.com/fsck-repair-file-system-errors-in-linux/)

system utility to check file system automatically (during boot time) or manually with command

- `sudo fsck /dev/sda`
- `sudo fsck -n /dev/sda`, will not do any change (just checking)

## df

[Reference](https://www.geeksforgeeks.org/df-command-in-linux-with-examples/)

check disk space usage,
examples:

- `df -h`
- `df -h --total`

## top

check cpu/memory usage

## dmesg

[Reference](http://www.linuxnix.com/what-is-linuxunix-dmesg-command-and-how-to-use-it/)

## lsof

list open files, use to find which process is holding what file open

- `lsof /media/INTENSO`, list opened files in path /media/INTENSO

## mount/umount

[mount](https://www.man7.org/linux/man-pages/man8/mount.8.html)

[umount](https://linux.101hacks.com/unix/umount/)

mount/unmount a filesystem

- `mount -a`, mounts all filesystems mentioned in file /etc/fstab
- `mount -l`, list mounted filesystems
- `umount /media/pi/INTENSO`, unmount a filesystem (USB device INTENSO)

## administer linux users

- `less /etc/passwd`, list linux users
- `sudo passwd tkosmop`, set password for user `tkosmop`
- `sudo useradd tkosmop`, add user `tkosmop` [ref](https://www.tecmint.com/add-users-in-linux/)
- `chage -l tkosmop`, displays last password set time

## less

- `less +?STRING_TO_SEARCH FILE`, search STRING_TO_SEARCH from the begging of FILE (press `n`, `Shift+n` to navigate between results)
- `less +/STRING_TO_SEARCH FILE`, search STRING_TO_SEARCH from start of FILE (press `n`, `Shift+n` to navigate between results)

## curl

[Reference](https://www.mit.edu/afs.new/sipb/user/ssen/src/curl-7.11.1/docs/curl.html)

- request <https://etssdev.xoas.inbroker.gr/vendor.js> with params
  - -v [shows request/response headers]
  - -k [ignores certificate errors]
  - -x "..." [use proxy ...]
  - `> /dev/null` [does not display output]
  - -u XXXX:YYYY [XXXX=server_user, YYYY=server_password]
  - -o "NAME.EXT" [save to file NAME.EXT]
  - -X [set http verb] `-X POST`

```bash
 curl -v -k -x "http://proxy.companyname.gr:8080" https://etssdev.xoas.inbroker.gr/vendor.js > /dev/null
```

- request with authorization headers

```bash
curl -k 'https://etssuat-admin.enexgroup.gr/ETSSUAT/mojsws/etssws/referencePrices?exchangeId=251&date=2020-09-02' \
  -H 'Connection: keep-alive' \
  -H 'Pragma: no-cache' \
  -H 'Cache-Control: no-cache' \
  -H 'Accept: application/json' \
  -H 'DNT: 1' \
  -H 'authorization: Bearer eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCIsIng1dCI6Imw4UUo0eERvOWRqNkRkbXhTd0JNRUM0RjEzOCIsImtpZCI6Im9yYWtleSJ9.eyJtYWlsIjoicHBjX3RyYWRlcjJAbWFpbC5jb20iLCJzdWIiOiJwcGNfdHJhZGVyMkBtYWlsLmNvbSIsIm9yYWNsZS5vYXV0aC51c2VyX29yaWdpbl9pZCI6InBwY190cmFkZXIyQG1haWwuY29tIiwib3JhY2xlLm9hdXRoLnVzZXJfb3JpZ2luX2lkX3R5cGUiOiJMREFQX1VJRCIsImlzcyI6Ind3dy5vcmFjbGUuZXhhbXBsZS5jb20iLCJsYXN0bmFtZSI6IlBQQ19UUkFERVIyIiwiZmlyc3RuYW1lIjoiUFBDX1RSQURFUjIiLCJvcmciOiJQUEMiLCJvcmFjbGUub2F1dGguc3ZjX3BfbiI6Ik9BdXRoU2VydmljZVByb2ZpbGUiLCJpYXQiOjE1OTg5Njk1NTEsIm9yYWNsZS5vYXV0aC5wcm4uaWRfdHlwZSI6IkxEQVBfVUlEIiwib3JhY2xlLm9hdXRoLnRrX2NvbnRleHQiOiJyZXNvdXJjZV9hY2Nlc3NfdGsiLCJjbGllbnQtbWFwcGluZyI6InB1YmxpYyIsImV4cCI6MTU5ODk3MzE1MSwiY2xpZW50LWFwcC1pZCI6IjRhYzcxZTViYjhiYzQyOWViNzVkNTAxZmEyYzY1Y2FmIiwidXNlcnJvbGVzIjoiY249TFJTX1RSQURFUixjbj1ERVZfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPUVUU1NfU0VSVkVSX0FQSSxjbj1ERVZfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPUVUU1NfV0VCVFJBREVSX1VTRVIsY249VUFUX0VOWF9FVFNTLG91PVNlcnZpY2VzLGRjPWVuZXhncm91cCxkYz1zdWJzOyBjbj1VU1NfVFJBREVSLGNuPURFVl9FTlhfRVRTUyxvdT1TZXJ2aWNlcyxkYz1lbmV4Z3JvdXAsZGM9c3ViczsgY249TFJTX1RSQURFUixjbj1VQVRfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPUVUU1NfV0VCVFJBREVSX1VTRVIsY249REVWX0VOWF9FVFNTLG91PVNlcnZpY2VzLGRjPWVuZXhncm91cCxkYz1zdWJzOyBjbj1SRUdfTk9NX1RSQURFUixjbj1VQVRfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPUVUU1NfU0VSVkVSX0FQSSxjbj1VQVRfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPVJFU0xSX1RSQURFUixjbj1VQVRfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPVRSQURFUixjbj1ERVZfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPVJFR19OT01fVFJBREVSLGNuPURFVl9FTlhfRVRTUyxvdT1TZXJ2aWNlcyxkYz1lbmV4Z3JvdXAsZGM9c3ViczsgY249VVNTX1RSQURFUixjbj1VQVRfRU5YX0VUU1Msb3U9U2VydmljZXMsZGM9ZW5leGdyb3VwLGRjPXN1YnM7IGNuPVJFU0ZJVF9UUkFERVIsY249VUFUX0VOWF9FVFNTLG91PVNlcnZpY2VzLGRjPWVuZXhncm91cCxkYz1zdWJzOyBjbj1UUkFERVIsY249VUFUX0VOWF9FVFNTLG91PVNlcnZpY2VzLGRjPWVuZXhncm91cCxkYz1zdWJzOyBjbj1SRVNGSVRfVFJBREVSLGNuPURFVl9FTlhfRVRTUyxvdT1TZXJ2aWNlcyxkYz1lbmV4Z3JvdXAsZGM9c3ViczsgY249UkVTTFJfVFJBREVSLGNuPURFVl9FTlhfRVRTUyxvdT1TZXJ2aWNlcyxkYz1lbmV4Z3JvdXAsZGM9c3VicyIsInBybiI6InBwY190cmFkZXIyQG1haWwuY29tIiwianRpIjoiZTVkZjk1NWItYmUwYS00OTMzLTg3ODYtZmQ1ZDk5ZTFkNWJlIiwib3JhY2xlLm9hdXRoLnNjb3BlIjoib3BlbmlkIiwib3JhY2xlLm9hdXRoLmNsaWVudF9vcmlnaW5faWQiOiI0YWM3MWU1YmI4YmM0MjllYjc1ZDUwMWZhMmM2NWNhZiIsInVzZXIudGVuYW50Lm5hbWUiOiJEZWZhdWx0RG9tYWluIiwib3JhY2xlLm9hdXRoLmlkX2RfaWQiOiIxMjM0NTY3OC0xMjM0LTEyMzQtMTIzNC0xMjM0NTY3ODkwMTIifQ.bWolsJyxGiOOEQD2GFbVhCxnmquPTY3-bX1_hue1KuNSodkJnJqWS9MpL1L_IqISWCYXOvt8Ip61r5T-X3gu2XWu9eAGMDNioeN_EolW8xvMVTj90RaAGpS3SWNydyldQgSs1Lgdw85za_SlIGLwy17jdKf-vnblrhs6bkRiDAI' \
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36' \
  -H 'Sec-Fetch-Site: same-origin' \
  -H 'Sec-Fetch-Mode: cors' \
  -H 'Sec-Fetch-Dest: empty' \
  -H 'Referer: https://etssuat-admin.enexgroup.gr/index.html' \
  -H 'Accept-Language: en-US,en;q=0.9,el;q=0.8' \
  --compressed
```

## wget

- ```wget https://github.com/VTsantilis/aksearch/archive/master.zip```, download file

## vi editor

- **search** by pressing `/`  then type string to search and press enter, navigate through search results by pressing `n` & `Shift + n`

## ln

- ```ln -s /path/to/folder/ symbolic-link-name```, create symbolic link (shortcut) with name symbolic-link-name from current directory to folder /path/to/folder/

## pkill

[Reference](https://linuxize.com/post/pkill-command-in-linux/)

- ```pkill firefox```, gracefully stop all firefox processes

## which

- ```which google-chrome```

## scp

[Reference](https://www.tecmint.com/scp-commands-examples/)

- `scp -i latramis.pem /var/www/json.php ubuntu@54.229.54.53:/var/www/html/test`

  copy local file json.php to remote server through ssh with public key

## cp

- cp -R path_to_source path_to_destination/

  copy folder `path_to_source` to folder `path_to_destination`. if folder `path_to_destination` does not exist, it will be created.

## zip

- `zip -r zipped_file path_to_folder/`, zip folder path_to_folder (recursively: -r) into file zipped_file

## cut

[Reference](https://www.computerhope.com/unix/ucut.htm)

for each line print value of column 14(sc-status), each line is splitted into columns with delimeter " " (space character)
input file: ex201207.log

```bash
cut -d " " -f 14 ex201207.log
```

## sort

[Reference](https://www.computerhope.com/unix/usort.htm)

## uniq

[Reference](https://www.computerhope.com/unix/uuniq.htm)

## awk

[Reference](https://www.computerhope.com/unix/uawk.htm)

**Utilize awk in IIS logs with following line format:**

```bash
date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
```

select lines with value 403 in column 14(sc-status), each line is splitted into columns with delimeter " " (space character), input file: ex201207.log, output file: /c/logs.txt

```bash
awk -F" " '$14 == "403"' ex201207.log > /c/logs.txt
```

print column 12(cs[User-Agent]), each line is splitted into columns with delimeter " " (space character), input file: ex201207.log, output file: /c/logs.txt

```bash
awk -F" " '{print $12}' ex201207.log > /c/logs.txt
```

select lines with value initSession in column 6(cs-uri-stem) and print only columns 12(cs[User-Agent]) & 6(cs-uri-stem),
input file: ex201208.log, output file: /c/ir_logs_referer_initsession.txt

```bash
awk -F" " 'match($6,/initSession/) {print $12, $6}' ex201208.log | sort | uniq -c > /c/ir_logs_referer_initsession.txt
```

select lines with value 403 in column 15(sc-status) and value like 195.78.86 in column 11(c-ip) in files with name pattern ex2012*.log (e.g. ex201201.log, ex201202.log etc)

```bash
awk -F" " 'match($15,/403/) && match($11,/195\.78\.86/)' ex2012*.log
```

search in IIS log for requests with http status (sc-status) 404
log file fields: date time cs-method cs-uri-stem cs-uri-query cs-username c-ip cs(User-Agent) cs(Referer) sc-status sc-bytes time-taken
sc-status is field number 10

```bash
awk -F" " -v str="404" 'index($10, str)' ex210208.log
```

case insensitive search for string `ping` in log field `cs-uri-stem`
log file fields: date time s-sitename s-computername s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes time-taken
cs-uri-stem is field number 7

```bash
awk -F" " -v str="ping" 'index(tolower($7), tolower(str))' ex210208.log
```

## id

[Reference](https://www.computerhope.com/unix/uid.htm)

get user `teo` uid

```id -u teo```

## fuser

[Reference](https://www.computerhope.com/unix/fuser.htm)

kill process that has binded tcp port 5000

`fuser -v -k -n tcp 5000`