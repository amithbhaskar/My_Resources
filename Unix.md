# Unix
Last Updated on : 24 May 2019

###  FILE PERMISSIONS / FOLDER
Create a folder with all permissions                           : mkdir -m 777 dirName
Create a file with all permissions                                                 : touch filename.txt && chmod 755 filename.txt
Change permisssions of all files in directory : chmod -R 777 /dirName/abc/def
Delete Empty Folders in a directory                                            : find . -type d -empty -delete
Remove files except a pattern                                                       : rm -v !(*Jun*)
Move files in subfolders to current Directory : find . -type f -mindepth 2 -exec mv -t ./ {} + [Note that that command will overwrite any files with the same name.]
Rename a file                                                                                                                       : mv /path/to/file.txt /path/to/file.xml
Creating multiple directories at once                         : mkdir myfolder{1,2,3} (Ex.myfolder1, myfolder2, and myfolder3)
Merge Multiple files to 1 file                                                         : cat file1 file2 file3 >> Outputfile
Find for location of a file                                                                 : find / -iname 'filename*'
Count number of Files in a directory                          : ls filename* | wc -l
List only directories in a path                                         : find /nz/path/ -type d  -ls
cp -u
--update              Copy only when the source file is newer than the destination file or when the destination file is missing.
Get unique list of dates in filenames                          : ls FileName.dat.2016* | cut -d '.' -f3| cut -c 1-8| sort -u
https://unix.stackexchange.com/questions/41550/find-the-total-size-of-certain-files-within-a-directory-branch

###  INSIDE VI EDITOR
Replace Ctrl-M characters                            : %s/^M//g

###  FOR LOOPS
View 1st line in all txt files in a folder                       : for i in `ls *.txt`;  do  echo $i;  cat $i|head -1;  done;
Remove Ctrl-M characters from Multiple files      : for i in `ls *.txt`;  do  echo $i;  sed 's/^M//g' $i >> ab_$i; mv ab_$i $i;  done;

###  GREP COMMANDS
Find text string in multiple files                                                  : egrep 'string' FileName*
Find fixed text string in multiple files                       : grep -F  "****" FileName.txt
Find text string in multiple subdirectories              : grep -r 'xyz' FileName
Find symbol asterisk “*”                                                                                                : grep '[*]' FileName.txt
View Line Number of string                                                                                          : grep -n "string" FileName.txt
View complete Line using Line Number ex.9                         : sed -n '9p' FileName.txt
View File Names containing string                                                             : grep -l 'string' FileName*.csv
Find for AAA and BBB and CCC (in that order)      : sed '/AAA.*BBB.*CCC/!d' FileName.txt
Find text string in Zipped files                                                     : zgrep 'string' file*.zip
Find multiple text strings from a text file                : grep -f strings.txt filename.txt > output.txt

###  REMOVE / REPLACE COMMANDS
Remove CTRL-M characters from a Single file in UNIX       : sed -e "s/^M//" FileName.txt > ab
Remove CTRL-@ characters from a Single file in UNIX      : tr < file-with-nulls -d '\000' > file-without-nulls
Remove Blank lines                         -> with awk                                                                                         : awk -F, 'length>NF+1' FileName.csv >text.txt
                                                                                                -> Multiple for files                                          : for i in `ls *.txt`;  do  echo $i;  awk -F, 'length>NF+1'  $i >> ab_$i; mv ab_$i $i;  done;
Remove last column in a file                                                                                                        : cat STG_EW_ISR_GIT_20180315000000.txt | cut -d '|' -f1-7  > test.txt
Remove NewLine Expr within doubleQuotes                                                        : gawk -v RS='"' 'NR % 2 == 0 { gsub(/\n/, "") } { printf("%s%s", $0, RT) }' filename > outputFile    
Remove Double Quotes in a file                                                                                                 : sed 's/\"//g' FileName.txt > NewFileName.txt
Remove New Line Expressions within double quotes                        : sed 's/||/\|/g' FileName | awk 'NR==1{printf "%s",$0; gsub(/[^|]/,""); nlast=n=length($0); next;} nlast==n{printf "\n";nlast=0} {printf "%s",$0; gsub(/[^|]/,""); nlast+=length($0)} END{print ""}' > NewFileName
                                                                                                                                                                OR -> with awk  : gawk -v RS='"' 'NR % 2 == 0 { gsub(/\n/, "") } { printf("%s%s", $0, RT) }' FileName
Remove comma within double quotes                                                                                    : sed -e 's/\(\"[^",]\+\),\([^",]*\)/\1 \2/g' -e 's/\"//g' FileName
Delete the first 10 lines of a file                                                                                  :  sed '1,10d' FileName
Replace space in filename with underscore                          : for f in *\ *; do mv "$f" "${f// /_}"; done
Replace space with underscore for header only  : sed -i -e '1s/ /_/g' FileName.txt
Replace CSV to csv in filenames                                                                 : for f in *CSV*; do mv "$f" "${f//CSV/csv}"; done
Replace Newline to '$' row delimiter                                       : for file in $1*  do  tr '\n' '$'  < $file >temp.$$ && mv temp.$$ $file       done
Replace ) with underscore in 1st line                                        : sed -i -e '1s/(/_/g' test.txt
Replace ( with underscore in 1st line                                        : sed -i -e '1s/(/_/g' test.txt
Replace & with underscore in 1st line                                      : sed -i -e '1s/&/_/g' test.txt

###  VIEW / COUNT COMMANDS
Cat Command Display Line Numbers                       : cat -n fileNameHere
View recent ran commands (Keyword)   : history | grep Keyword
View Ctrl-M characters                                                                  : cat -tv FileName.txt
View top N records in a txt file                    : cat FileName.txt | head -1
                                                or using sed ->                                   : sed 10q FileName.txt
View bottom N records in a txt file                            : cat FileName.txt | tail -1
                                                or using sed ->                                   : sed -e :a -e '$q;N;11,$D;ba'  FileName.txt OR sed '$!d'  FileName.txt
View last but 1 line using sed                                      : sed -e '1{$q;}' -e '$!{h;d;}' -e x  FileName.txt                                     
View particular column ex.8th in csv        : awk -F "\"*,\"*" '{print $8}' FileName.csv
Sort distinct values of 8th col in csv          : cut -d ',' -f8 FileName.csv | sort | uniq
Count of unique values in column                             : cat FileName.txt|cut -d '|' -f2| sort -u |wc -l
Count number of delimiters                                                         : head -1 FileName.txt | tr "||" '\n' | wc -l
Count number of delimiters in rows                         : cat FileName* | awk '{ FS = "," } ; { print NF}'|sort -u
Count Line/Word/Byte/Characters                           : wc -l -w -c -m tecmint.txt
Run a shell script                                                                                              : sh FileName.sh
Run a command in background                                  : nohup abcd &
Unzip a zip file                                                                                   : unzip *.zip
Clears Entire line                      : Ctrl+U
Clears line till end of the line       : Ctrl+K
Clears the word before the cursor       : Ctrl+W
Search commands history                 : Ctrl+R
Move back to last working directory     : cd - 
View only lines which match reg expr      : sed -n '/regexp/p' FileName.txt OR  sed '/regexp/!d' FileName.txt
View lines of 65 characters or longer       : sed -n '/^.\{65\}/p' FileName.txt
View lines of 65 characters or lesser        : sed -n '/^.\{65\}/!p' FileName.txt OR sed '/^.\{65\}/d' FileName.txt
View lines less than 11 columns                                 : cat FileName.txt | awk -F, 'NF>11 && $NF!=""'
Count rows starting with 'T'                                         : grep -E '^(T)' FileName1 FileName2 | wc -l
Count list of numbers                                                                    : cat numbersFile.txt | awk '{total = total + $1}END{print total}'

###  SFTP COMMANDS:
Check for SSH Connection                                                                                                            : ssh un@ip
                -> if connection is not on default port(i.e. 22) : sftp -oPort=custom_port_number un@ip
Copy file to local directory                                                                                            : get filename.txt
Copy full directory and all of its contents                                : get -r someDirectory
Copy & maintain permissions/access times                                            : get -Pr someDirectory
Rename file while copy                                                                                                                   : get remoteFile localFile
Transfer Local Files to the Remote System                                            : put localFile
Copy files from current folder to remote folder    : scp filepattern un@ip:/pathToFolder
To get Locals working directory in ssh/sftp                                           : lpwd

###  SHELL PROCESSES
Running processes in DS server  : ps -ef | grep 'FileName.sh'
To kill running processes                              : kill -9 PIDnumber
To see Zombie Processes                                              : ps -xal
Available free sizes                                          : df -h
Directories of sizes                                          : du -h

###  TEXT FILTERS / SUBSTITUTION:
Cut till delim ':'                                                                  : cut -d ':' -f2 FileName.txt > NewFileName.txt
Add commas to numeric strings                                 : sed -e :a -e 's/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/;ta'  (Ex."1234567" to "1,234,567")
Find duplicate in particular column           : cat FileName.txt |cut -d '|' -f2| sort | uniq -d
Find full row duplicates in a file  : sort FileName.txt | uniq -cd
Find duplicates in certain columns            : awk -F '|' '!seen[$1 $2]++' FileName.txt > test1.txt
Find the maximum value in the file           : cat FileName.txt | cut -d '|' -f3 | sort -nr | head -1

###  Date Manipulations
add X days to date                                                          : prog_end_date=`date '+%C%y%m%d' -d "$end_date+10 days"`

### check if there were no arguments provided
```
if [ $# -eq 0 ]; then
    echo "No arguments provided"
    exit 1
fi
: '
```

## Tee 
Tee is normally used to split the output of a program so that it can be both displayed and saved in a file.

## Cron Service 
The cron daemon, or crond, stays dormant until a date/time specified in one of the config files, or crontabs which enables unix users to execute commands or scripts to run automatically at a specified time/date, either as one-time events or as recurring tasks.
To find out if its running : ps aux | grep crond
List all your cron jobs      : crontab -l
https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/
https://kb.iu.edu/d/afiz

## SED:
SED command in UNIX is stands for stream editor and it can perform lot’s of function on file like, searching, find and replace, insertion or deletion. SED command in unix supports regular expression which allows it perform complex pattern matching.
sed treats multiple input files as one long stream. The following example prints the first line of the first file (one.txt) and the last line of the last file (three.txt). Use -s to reverse this behavior.
sed -n  '1p ; $p' one.txt two.txt three.txt
There are four parts to this substitute command: sed 's/day/night/' old.txt > new.txt
s                                              Substitute command
/../../                     Delimiter(can use _ or \ or :)
one                                        Regular Expression Pattern Search Pattern
ONE                                       Replacement string
An exit status of zero indicates success, and a nonzero value indicates failure. GNU sed returns the following exit status error values:
1 - Catchall for general errors
2 - Misuse of shell builtins (according to Bash documentation)
https://shapeshed.com/unix-exit-codes/
Cheat sheet : https://gist.github.com/LeCoupa/122b12050f5fb267e75f

### Line Operations
^ denotes beginning of line
$ represents end of the line
sed 's///g' - s denotes substitution
sed -i '///' filename.txt : denotes modifying the original file
sed 's/[0-9]/*/g' filename.txt : subtitutes * in place of any number
sed 's/[^0-9]/*/g' filename.txt : subtitutes * in place of any non-number
sed 's/[A-z]/*/g' filename.txt : subtitutes * in place of any alphabet(Capitals come first is ascii range)
sed 's/[A-z]ind/*/g' filename.txt : subtitutes * in place of any alphabet followed by a letter 'i'.
sed 's/[A-z0-9]/*/g' filename.txt : subtitutes * in place of any alphanum
sed 's/[0-9]/(&)/g' : Using an Ampersand to to surround matched string with '()'
sed 's///g' : / is a delimiter, so if forward slash (/) exists in the file that needs substitution, place a backslash(\) before the foeward slash before ur string containing the fwd slash-(/myString).
Ex: sed 's/\/myString/ReplaceString/g' filename.txt
we can also use '_ or : or anyChar' as a delimiter in sed as : sed 's_mystring_ReplaceStr_g' filename.txt
sed 's///g;s///g;s///g' filename.txt : can contatenate multiple seds
sed 's/\w*/ReplaceString/g' filename.txt : will replace the first alphanum word in each line.
sed 's/\w$/ReplaceString/g' filename.txt : will replace the last alphanum word in each line.
sed 's/\bMyString\b/ReplaceString/Ig' filename.txt : will replace the full word match of MyString in each line.
sed '/myStr/D' filename.txt : will delete all lines that match myStr.
sed -n 's/myString/ReplaceString/pg' filename.txt : shows only strings that have matched result lines.
sed 's/myString/ReplaceString/Ig'  filename.txt : ignores lower or uppercase letters.
sed '4, q' filename.txt : display first 4 lines and quit.
sed '2!{/^#/d}' filename.txt : skip first 2 lines and delete all lines starting with #.

## Trap
Trap is a function built into the shell that responds to hardware signals and other events.

## DF Command
The df command is a command line utility for reporting file system disk space usage. It can be used to show the free space on a Unix or Linux computer and to understand the filesystems that have been mounted. It supports showing usage in Bytes, Megabytes and Gigabytes
This show six columns as follows
- Filesystem - The filesystem on the machine
- 1K-blocks - the size of the filesystem in 1K blocks
- Used - the amount of space used in 1K blocks
- Available - the amount of available space in 1K blocks
- Use% - the percentage that the filesystem is in use.
- Mounted on - where the filesystem is mounted

## View disk space in human readable format we pass the -h option and to print in block-size=1K.
`$ df -kh`
## Detect Shell
echo $0 #shows which shell(command interpreter) is being used, we can change shell by using sh command (used for emails)

## Shebang #! is called shebang OR hashbang
                It specifies the path to the command line interpreter Ex: #!/bin/bash
				
## Wildcards:
                ? : lists one character wildcard, eg: filename? will output: filename1 filename2 etc
                ; : semicolon represents synchronus excecution. Single line multiple cmd excecution.
                {}: braces is used for variable substittution Eg: echo $(Var)ing outputs Learning where $Var = Learn.
                # : is a comment
                & : puts a cmd excecution in background. Eg: & sleep 5 &                               puts the script to run in the background to sleep for 5 seconds without losing control over the cmd prompt.
                | : stiches commands together.
## Quotes:
                `date` : back ticks are used to write and run inline keywords. New line characters are not preserved and arrays are printed in a single line.
                Single Quote : Prints as it is. (Strong quotes -> does not interpolates variables)
                                                                   Backslash \ : Is used to print single quote in a string.
                Double Quotes: Ignores wildcards and translates all inline keywords. (Weak quotes -> interpolates variables). New line characters are preserved.
                                                                                Use single quote to print doubleQuotes
                \r\n
                \'
## Special Characters:
                \b : backspace
                $0 : displays name of the script (filename)
                END : called Here document, Usually used like multiple echo statements.
```
"                                              Eg: cat <<END This space can be used
                                                                                to display information
                                                                                to the user from inside the script.
                                                                                END
"                                              Eg: #Script to count lots of words at a time, Outputs 16
"                                                              wc -w <<EOF
                                                                                                This is a test.
                                                                                                Apple juice.
                                                                                                100% fruit juice and no added sugar, colour or preservative.
                                                                                                EOF
"                                              Eg: # write an email to admin
"                                                              mail -s 'Backup status' vivek@nixcraft.co.in<<END_OF_EMAIL
                                                                The backup job finished.
                                                                End date: $(date)
                                                                Hostname : $(hostname)
                                                                Status : $status
                                                                END_OF_EMAIL
"                                                             
```

### Remove blank lines in a file
grep -v '^$' filename >> new_filename

## Variables:
		let : let command performs the calculation and then stores in the variable Eg: let a=16+5 outputs a=21 else it will outputted as a='16+5'
		-r : Variable is made read only variable and cannot be updated.(was previously used as typeset instead of declare) Eg: declare -r var=1.
		-i : Variable is in Integer mode. Eg: declare -i var
		unset $var : destroys $var from the memory.
		$* - displays all arguments
		$# - displays number of arguments passed
		$0 - name of the file
		$? - capture exit code of a script
		break - stops the loop
		continue - skips excecuting commands in that iteration and proceeds with next iteration

## Conditional Operators:
                -gt
                -lt
                ==
                !=
                && : and operator bw 2 conditions
                || : or operator bw 2 conditions
                if [:] : to create infinite loops which can be broken based on necessity
                -z : zero length sting check
                -n : non zero length sting check
                -r : returns true if parameter is readable (readable permisssions) Eg: if [ -r $a ] then echo 'a is readable' fi
                -d : returns true if parameter is a directory
                -f : returns true if parameter is a file

## Control Structures:
if [$a == $b]
then
                echo 'a equal to b'
elif [$a -gt $b]
then
                echo 'a greater then b'
else
echo 'a less than b'
fi

## Looping Structures:
While Loop displays all sh files
```
for a in *.sh
do
cat $a
done
#
while [$a -gt 0]
do
                echo ''
done
#loop runs till a=11 similar to do while loop
until [$a -gt 10]
do
                a=$(($a +1))
                echo '$a=' $a
done
```

### Error
bash file.sh > successful.log 2>error.log - output redirection to success log and error(code 2) log
bash file.sh >> successful.log 2>>error.log - output redirection appends in the existing file
bash file.sh > successful.log 2>&1 - output redirection to same file
file1 < file5 > op.log - file5 is input for file1 and saves output to op.log

### Arrays
array[0] - represents zeroth element. (Running cmd - array - also will output zeroth element.)
array[i]:n  - prints i''th element''s nth character onwards.
{#array[*]} OR {#array[@]} - prints the number of elements in the array

### Signals 
- used for inter process communication. 
2 kinds : Maskable(sigint)/Non Maskable(kill -9 is a sigkill signal).
kill -l : lists all signals
Ctrl+C can be masked using SIGINT signal

###  Simple file load with a Variable
```
[nz@webbia1 archive]$ cat LoadStoreInvDeltas.sh
echo STARTING LOAD for `date`
for i in `ls -1 /nz_backup/archive/srs_bnfeed_store_inventory.dat.$1*`
do
        echo $i `date`
        nzload -db test -t WORK_STORE_INV_STAGE -df $i -delim '|' -maxerrors 100
done
echo FINISHED LOAD `date`
```

###  Accurate Line Count in Zipped files
```
for i in  `find /pathName/fileName*`
do
rec_count=`cat  $i | zcat | wc -l`
echo "$i : $rec_count"
done
```

### Run code for specific dates:
```
DATE=$1
start_date_id=`date '+%Y%m%d' -d "$DATE-540days"`
end_date_id=`date -d $DATE +'%Y%m%d'`
echo $start_date_id
echo $end_date_id
date_list=`nzsql -t -A -c "select distinct date_id from spsales_dds_new..dim_time d where date_id between $start_date_id   and $end_date_id order by 1"`
echo "Starting to Load Daily Store Inventory Deltas"
echo $date_list
for i in $date_list
do
echo "Loading Daily Store Inventory Deltas for $i"
        vLast_Snapshot_date_id=`nzsql -t -A -c "select max(snapshot_date_id) from spsales_dds_new..dim_time where date_id < $i;"`
echo " to $vLast_Snapshot_date_id"
done
```

### Difference of two variables
num1="$(($num1+$num2))"
OR
expr $num1 + $num2
## Shell script common template 
```
#!/bin/bash
# ------------------------------------------------------------------
# Title                                                    :
# Description                     :
# Usage                                                : command -ihv args"
# Author                                              :
# ------------------------------------------------------------------
# My Generic BASH script template
#
version="1.0.0"               # Sets version variable
#
scriptTemplateVersion="1.3.0" # Version of scriptTemplate.sh that this script is based on
#                               v.1.1.0 - Added 'debug' option
#                               v.1.1.1 - Moved all shared variables to Utils
#                                       - Added $PASS variable when -p is passed
#                               v.1.2.0 - Added 'checkDependencies' function to ensure needed
#                                         Bash packages are installed prior to execution
#                               v.1.3.0 - Can now pass CLI without an option to $args
#
#
# ##################################################
# --- Locks -------------------------------------------------------
# --- Body --------------------------------------------------------
#  SCRIPT LOGIC GOES HERE
echo
# --- Logging --------------------------------------------------------
# -----------------------------------
# ##################################################
# ----------------Here Document -----------------------------------
# cat <<END This space can be used
                                                                                to display information
                                                                                to the user from inside the script.
                                                                End date: $(date)
                                                                Hostname : $(hostname)
                                                                Status : $status
                                                                                END
# ##################################################                                                                
# ############# ############# #############
# ##       TIME TO RUN THE SCRIPT        ##
# ##                                     ##
# ## You shouldn't need to edit anything ##
# ## beneath this line                   ##
# ##                                     ##
# ############# ############# #############
```

###  Find out :
- Purge files older than 30 days
- after ftp check file size

###  LINKS:
https://www.fastwebhost.in/blog/putty-30-useful-putty-commands-for-beginners/
http://www.grymoire.com/Unix/Sed.html
