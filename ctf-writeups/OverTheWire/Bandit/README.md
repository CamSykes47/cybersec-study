# Bandit
> Unix/Linux Basics


## Lvl 0 -> bandit0

## Lvl 1 -> ls -> cat [file] -> ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

## Lvl 2 -> ls -> cat ./[file] -> 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

## Lvl 3 -> ls -> cat ./"[file]" -> MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

## Lvl 4 -> ls -> cd [directory] -> find -> cat ./[file found] -> 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

## Lvl 5 -> ls -> cd inhere -> ls -l (-l lists file values, -la lists them with clearer readouts) -> file ./* (* acts as a 'wildcard value';cmd  essentially pulls all files whose names start with characters preceding *) -> find ASCII text file -> cat ./[file] -> 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

## Lvl 6 -> ls -> cd inhere -> ls -la -R (shows all files in subdirectories with exact sizes) -> find -type f -size 1033c (finds data that is both a file and is 1033 bytes big) -> find -type f -size 1033c ! -executable (finds file that is 1033 bytes large and non-executable -> find -type f -size 1033c ! -executable -exec file '{}' \; | grep "ASCII text" (finds nonexecutable file of size 1033, gets the file data type via file command, and filters for the file type "ASCII text" -> cat [file] -> HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

## Lvl 7 -> ls -a (check what's in home directory) -> find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null (recursively searches for file that's owned by set user, owned by set group, of size 33 bytes, and disregards error messages) -> cat [file] -> morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

## Lvl 8 -> ls -a -> cat file | grep millionth (outputs the data line containing the word "millionth") -> dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

## Lvl 9 -> ls -> sort [file] | uniq -u (need to sort data in file so that uniq can output the line that appears only once) -> 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

## Lvl 10 -> ls -> strings [file] (looks for human-readable strings/sequences of printable characters in a file) -> strings [file] | grep = (searches for human readable strings in file starting with =) -> FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

## Lvl 11 -> ls -> base64 [file] (produces encoded data) -> base64 -d [file] (decodes encoded data and outputs it) -> dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

## Lvl 12 -> ls -> strings data.txt | tr 'N-ZA-Mn-za-m' 'A-Za-z' (reverses ROT13 using tr and prints the string data of the file) -> 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4 

## Lvl 13 -> ls -> cd /tmp (move to the /tmp directory where we will have permissions to make and alter directories) -> mkdir [unique directory name] OR mktemp -d (creates either a directory with a custom unique name or a randomly generated unique name) -> cp data.txt [directory name] -> cd [directory name] (copy data from data.txt file to new directory) -> mv data.txt [new file name] (moves data from file into a newly created file; can adjust suffix to change file type) -> file [created file] (checks file type) -> xxd -r [created file] [new file for reverted data] (reverts hexdump process and applies reverted data to a new file) -> file [reverted file] (check file type for compression) -> (for gzip) mv [reverted data] [new filename].gz (creates a .gz file using reverted data, which can then be decompressed with gzip) -> gzip -d [reverted.gz file] (decompress with gzip) -> (for bzip2) mv [reverted data] [new filename].bz2 (creates a .bz2 file using reverted data, which can then be decompressed with bzip2) -> bzip2 -d [reverted.bz2 file] (decompress with bzip2) -> ls (check for decompressed filename) -> file [decompressed file] (check file type for compression) -> xxd [decompressed file] | head OR cat [decompressed file] (review data for strings or other identifiers) -> (for tar archives) mv [decompressed file] [decompressed file].tar (moves data to a tar archive so that it can be extracted) -> tar -xf [decompressed.tar] (extracts found file from tar archive) -> file [extracted file] (check file type and compression/archiving) -> ls (check for extracted file name)-> (repeat extraction, bzip2, and gzip methods until readable/ASCII file appears) -> cat [readable file] -> FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

## Lvl 14 -> ls (look for private key name & location) -> ctrl + d (exit bandit13) -> scp -P 2220 bandit13@bandit.labs.overthewire.org:[private key name] . (goes through bandit13, which we've already accessed, and downloads the file at the name/location we designated so that we can use it from our home directory) -> chmod 700 [downloaded key] (updates permissions for downloaded key so that only I can read, write, or execute the file) -> ssh -i [downloaded key file] -p 2220 bandit14@bandit.labs.overthewire.org (accesses server with the private key we downloaded and altered)

## Lvl 15 -> cat /etc/bandit_pass/bandit14 (password location for current level, provided previously) -> telnet localhost 30000 (access host location at provided port -> enter MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS -> 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

## Lvl 16 -> ncat --ssl localhost 30001 (use ncat to open access via SSL to host via port -> [enter lvl 15 password] -> kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

## Lvl 17 -> nmap -p31000-32000 localhost (check to see which ports are open in range) -> (OPTIONAL) nmap -p31000-32000 localhost | netstat -l (lets you know which ports are being listened to) -> nmap -p[openport1],[openport2],... localhost -sV (check previously found open ports for service version info to look for SSL; will take long) -> (EITHER) openssl s_client -quiet -connect localhost:[ssl port found] (OR) -> openssl s_client -ign_eof -connect localhost:[ssl port found] (access found port via SSL to obtain file data containing private key) -> copy RSA Private key text -> logout bandit16 -> nano [filename for key] (create a usable file for key; paste key into file and save) -> chmod 400 [key file] (make sure file only has read permissions for owner(you)) -> ssh -i [key file] -p 2220 bandit17@bandit.labs.overthewire.org (access server) -> yes to fingerprint

## Lvl 18 -> ls -> diff -b passwords.old passwords.new (shows brief view of differences between both files) -> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

## Lvl 19 -> logout -> ssh -p 2220 bandit18@bandit.labs.overthewire.org -> enter Lvl 18 provided password -> 'Byebye!' error (prick) -> ssh -p 2220 bandit18@bandit.labs.overthewire.org ls (find name of file in server) -> lvl 18 password -> ssh -p 2220 bandit18@bandit.labs.overthewire.org cat [filename] (print output of file before you get kicked) -> cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

## Lvl 20 -> logout -> setuid (need to download tool, allows setting username & password, as well as performing actions as that user) -> login to bandit19 -> ls -> ./bandit20-do (run command as bandit20) -> ./bandit20-do ls /etc/bandit_pass (look through files in /etc/bandit_pass for bandit20) -> ./bandit20-do cat /etc/bandit_pass/bandit20 (output password from bandit 20 for bandit 21) -> 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
