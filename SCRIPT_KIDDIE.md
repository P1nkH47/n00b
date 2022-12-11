## Playing around with .tar

Uncompressing a .tar (those are multiple cases)
```bash
tar xf file.tar.gz

#you can use that command on any kind of tarball (files with a .tar extension)

tar xf file.tar #like this one 
```

Compressing a directory into a tarball (.tar.gz in that case)

# MOVING AROUND (IMPORTANT)
###### So of course when u navigate a terminal u want to know how to navigate around.

Moving through directories

```bash
cd directory
```

If there is a subdirectory u can move to it like so:

```bash
cd directory/subdirectory
```

and so on 

```bash
cd directory/subdirectory1/subdirectory2/.../subdirectory9999
```
**(as long as it exists)**

***Some knowledge of the linux filesystem might prove useful:***

- **/ (root of the filesystem):** Primary hierarchy root and root directory of the entire file system hierarchy.
- **/bin :** Essential command binaries that need to be available in single-user mode; for all users, e.g., cat, ls, cp
- **/boot :** Boot loader files, e.g., kernels, initrd. (tbh u wont hang around here unless u fuck up)
- **/dev :** Essential device files, e.g., /dev/null
- **/etc :** Host-specific system-wide configuration files
-  **/home :** Users’ home directories, containing saved files, personal settings, etc
-  **/lib :** Libraries essential for the binaries in /bin/ and /sbin/
-  **/mnt :** Temporarily mounted filesystems
-  **/opt :** Optional application software packages 
-  **/sbin :** Essential system binaries, e.g., fsck, init, route
-   **/srv :** Site-specific data served by this system, such as data and scripts for web servers, data offered by FTP servers, and repositories for version control systems (NOT ALWAYS PRESENT)
-   **/tmp :** Temporary files. Often not preserved between system reboots, and may be severely size restricted (thats where u wanna download your linpeas and shit cuz the system wont mind you touching /tmp)
-    **/usr :** Secondary hierarchy for read-only user data; contains the majority of (multi-)user utilities and applications
-     **/proc :** Virtual filesystem providing process and kernel information as files. In Linux, corresponds to a procfs mount. Generally automatically generated and populated by the system, on the fly
-     . means current directory, .. means previous one, ~ means home directory (for example /home/scriptkiddie)


Creating directories (same shit than we cd except we make them, kinda):

```bash
mkdir directory_name

### CREATES A "directory_name" directory
```

You can also make a directory + subdirectories, you need to use the `-p` flag

```bash
mkdir -p directory/subdirectory
```

And like i showed you with cd you can just str8 up
```bash
mkdir -p directory/subdirectory1/subdirectory2 # and so on 
```

**To list the content of a directory**
`ls` will list the content of the current dir "."

You can select the directory u wanna show content of, like 
```bash
ls directory # will return the content of "directory"
```

a more detailed approach of ls would be using the `-lah` flag 

Just use ls like u normally would but add the flag if u want more details including dotfiles (hiddenfiles)

You can output the content of a file with **cat**
```bash
cat flag.txt
```

You can also search for a specific pattern in a file or stream of output 

For example if you are searching for **HTB** in **garbage.txt**
```bash
grep "HTB" garbage.txt
```

So here we have some pretty fancy stuff

FINDING SUID BINARIES FOR PRIVESC **LESFUCKINGOOOOO**

```bash
find / -perm -u=s -type f 2>/dev/null

# find is just the command
# / is basically for every file in the filesystem
# -perm -u=s we looking for the juicy stuff
# f is for file 
# 2>/dev/null is basically output every error to /dev/null so we dont get bombed with perm errors.
```

## BUT WHAT ABOUT HACKING? u may ask me 

> In your home directory, i recommend making some particular directories that might help you when doing a HTB box like a dir for files u download often from the machine or a dir with your scripts and what not 

**not really related but here is the structure of directories in my home dir**
- **drop** => thats where i store the files i often drop on the machine im attacking, shit like **chisel**, **linpeas**, **pspy64** down&exec payloads go there 
- **tools** => those fricking tools from github u might use, or even your custom made ones, store them there
- **lists** => thats where i store my wordlists.
- thats pretty much it but yeah, pretty useful 

> btw hacktricks go hard, check that out 

Well here u go:

**NMAP TCP initial scan** (the one u want to run first) 
```bash
nmap -sC -sV -oN nmap_output IP # where IP is either the IP or DOMAIN NAME of the machine u wanna scan

# -sC specifies defaults scripts
# -sV probes for each port to know what service is running
# -oN nmap_output =>  will output the nmap scan to nmap_output
# IP well thats the IP of the machine u wanna scan
```

**NMAP UDP scan**
(same shit but add the -sU flag for UDP) (run it to deepen the enum, rarely needed but u never know ie: u need that for UDP)
```bash
nmap -sU -sC -sV -oN nmap_output IP
```

You can scan every port by adding `-p-`

FFuF web directory
```bash
ffuf -u http://site.com/FUZZ -w lists/webdirs.txt
```

thats basic usage but it will fuzz webdirs (u can filter out and shit but eh for now i doubt u will need it or just ask me)

if u find an interesting dir u can just start the fuzzer again (u could use recursive mode but god damn it can be a pain if there is a lot of dirs)

```bash
ffuf -u http://site.com/interesting/FUZZ -w lists/webdirs.txt
```

FUZZ is basically the beacon that says to FFuF to smack shit from the list here 

**FFuF VHOST**
```bash
ffuf -u http://machine-ip -H "Host: FUZZ.machine.htb" -w lists/vhosts.txt
```
pretty straight forward

I like to crawl shit to find stuff fuzzer wouldnt normally 

just run gospider and you are golden 

```bash
gospider -s http://machine.htb
```

that might find some hidden stuff that u can add in your **/etc/hosts** so you can reach it.


