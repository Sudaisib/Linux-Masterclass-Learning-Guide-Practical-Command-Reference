# Chapter 2: File and Directory Management

> Module Focus: Navigating the Linux filesystem with confidence, understanding paths and file types, and mastering the core commands used to create, copy, move, view, and remove files and directories.

---

## Table of Contents

1. Introduction to File and Directory Management
2. Elevated Privileges: `sudo`, `su`, and `sudo su`
3. Knowing Where You Are: `pwd` and the Home Directory
4. Absolute Paths vs Relative Paths
5. Moving Around: `cd`
6. Understanding File Types
7. Creating Directories: `mkdir`
8. Creating Files: `touch`
9. Listing Contents: `ls`
10. Copying Files and Directories: `cp`
11. Moving and Renaming: `mv`
12. Removing Files and Directories: `rm` and `rmdir`
13. Writing to Files: `echo`
14. Viewing File Content: `cat`
15. Chapter Summary
16. Knowledge Check
17. What's Next

---

## 1. Introduction to File and Directory Management

Everything on a Linux system is organized as a file, whether it's a document, a directory, a device, or a running process's information. Before you can write scripts, manage servers, or deploy applications, you need to be completely comfortable moving through the filesystem and manipulating what's inside it. This chapter builds that comfort from the ground up, one command at a time.

---

## 2. Elevated Privileges: `sudo`, `su`, and `sudo su`

Some of the actions in this chapter, and many you'll meet later, require permissions beyond what a regular user account has. Linux gives you a few different ways to gain that access temporarily.

### 2.1 `sudo`

**Definition:** `sudo` (superuser do) runs a single command with root level privileges, then immediately returns you to your normal account. It is the safest and most commonly used method of privilege escalation because access is granted one command at a time and every use is logged.

**Syntax:**
```bash
sudo command
```

**Example:**
```bash
sudo apt update
```
This runs only the `apt update` command as root. Once it finishes, you're back to operating as your regular user.

### 2.2 `su`

**Definition:** `su` (substitute user, or switch user) starts a new shell session logged in as a different user, most commonly root. Unlike `sudo`, it does not return you automatically, you remain logged in as that user until you explicitly exit.

**Syntax:**
```bash
su username
```

**Example:**
```bash
su root
```
You'll be prompted for root's own password, and once entered, every subsequent command runs as root until you type `exit`.

### 2.3 `sudo su`

**Definition:** `sudo su` combines both commands: it uses your own account's sudo privileges to launch a root shell, without ever needing to know root's actual password. This is why `sudo su` is the far more common pattern in modern Linux administration, since many distributions disable the root password entirely.

**Syntax:**
```bash
sudo su
```

**Example:**
```bash
sudo su
```
You're prompted for your own password (not root's), and you land directly in a root shell, typically shown by a `#` prompt instead of `$`.

**Exiting a root shell**

**Syntax:**
```bash
exit
```

**Example:**
```bash
exit
```
This returns you to your original, non-privileged shell session.

---

## 3. Knowing Where You Are: `pwd` and the Home Directory

### 3.1 `pwd`

**Definition:** `pwd` (print working directory) outputs the full, absolute path of the directory you are currently standing in. It's the fastest way to confirm exactly where you are before running a command that depends on location, such as `rm` or `mv`.

**Syntax:**
```bash
pwd
```

**Example:**
```bash
pwd
```
Output:
```
/home/ibrahim/projects
```

### 3.2 The Home Directory

**Definition:** The home directory is the personal storage space automatically assigned to a user account when it's created, typically at `/home/username` on most distributions. It's where your personal files, configuration files, and downloads live by default, and it's the directory you land in the moment you log in.

**Referring to the home directory with `~`**

**Syntax:**
```bash
cd ~
```

**Example:**
```bash
cd ~
pwd
```
Output:
```
/home/ibrahim
```
The tilde (`~`) is a shortcut that always points to the current user's home directory, no matter where you are in the filesystem.

**Returning home from anywhere**

**Syntax:**
```bash
cd
```

**Example:**
```bash
cd
```
Running `cd` with no arguments at all has the exact same effect as `cd ~`, it always takes you home.

---

## 4. Absolute Paths vs Relative Paths

Understanding the difference between these two path types is one of the most important skills in this chapter, since nearly every command that follows accepts a path as an argument.

### 4.1 Absolute Path

**Definition:** An absolute path describes a file or directory's location starting from the root of the filesystem (`/`), regardless of where you currently are. It always begins with a forward slash and always points to the exact same location no matter your current directory.

**Syntax:**
```bash
/path/from/root/to/target
```

**Example:**
```bash
cd /home/ibrahim/projects/linuxcraft
```
This command works identically whether you're currently in `/home/ibrahim` or `/var/log`, because it doesn't depend on your starting point.

### 4.2 Relative Path

**Definition:** A relative path describes a location starting from your current directory rather than from the root. It never begins with a forward slash, and its meaning changes depending on where you are when you run it.

**Syntax:**
```bash
path/from/current/location
```

**Example:**
```bash
cd projects/linuxcraft
```
This only works if you're already in a directory that contains a `projects` folder, if you're anywhere else, it will fail.

**Special relative path symbols**

| Symbol | Meaning |
|---|---|
| `.` | The current directory |
| `..` | The parent directory, one level up |
| `../..` | Two levels up |
| `-` | The previous directory you were in (used with `cd`) |

**Example:**
```bash
cd ..
```
Moves you one directory up from wherever you currently are.

---

## 5. Moving Around: `cd`

**Definition:** `cd` (change directory) moves you from your current directory into a different one, using either an absolute or relative path.

**Syntax:**
```bash
cd /path/to/directory
```

**Examples:**
```bash
cd /var/log              # Absolute path, works from anywhere
cd projects               # Relative path, must be inside current directory
cd ..                     # Move up one level
cd ../..                  # Move up two levels
cd ~                      # Go straight to your home directory
cd -                      # Jump back to the previous directory you were in
```

---

## 6. Understanding File Types

**Definition:** Linux categorizes every item in the filesystem into a file type, which is visible as the very first character of a permission string when you run `ls -l`. Recognizing these types at a glance is essential for understanding what you're actually looking at.

| Symbol | File Type | Description |
|---|---|---|
| `-` | Regular file | Text files, scripts, binaries, images, and most everyday files |
| `d` | Directory | A folder that contains other files and directories |
| `l` | Symbolic link | A shortcut or pointer to another file or directory elsewhere |
| `c` | Character device | Represents hardware that transfers data one character at a time (e.g., a keyboard) |
| `b` | Block device | Represents hardware that transfers data in blocks (e.g., a hard disk) |
| `s` | Socket | Used for communication between processes |
| `p` | Named pipe | Allows one process to send data directly to another |

**Checking a file's type directly**

**Syntax:**
```bash
file filename
```

**Example:**
```bash
file myfile.sh
```
Output:
```
myfile.sh: Bourne-Again shell script, ASCII text executable
```

---

## 7. Creating Directories: `mkdir`

### 7.1 Basic `mkdir`

**Definition:** `mkdir` (make directory) creates a new, empty directory at the location you specify.

**Syntax:**
```bash
mkdir directoryname
```

**Example:**
```bash
mkdir projects
```

### 7.2 `mkdir -p`

**Definition:** The `-p` (parents) flag creates a full chain of nested directories in a single command, automatically creating any parent directory in the path that doesn't already exist, instead of failing with an error.

**Syntax:**
```bash
mkdir -p path/to/nested/directory
```

**Example:**
```bash
mkdir -p projects/linuxcraft/chapters/chapter2
```
Without `-p`, this command would fail immediately unless `projects`, `linuxcraft`, and `chapters` already existed.

### 7.3 Creating Multiple Directories at Once

**Definition:** `mkdir` accepts more than one directory name in a single command, creating each one independently at the same level.

**Syntax:**
```bash
mkdir directory1 directory2 directory3
```

**Example:**
```bash
mkdir images docs scripts
```
This creates three separate, independent directories in one step.

**Creating multiple nested directory structures at once with brace expansion**

**Syntax:**
```bash
mkdir -p parent/{dir1,dir2,dir3}
```

**Example:**
```bash
mkdir -p linuxcraft/{chapter1,chapter2,chapter3}
```
This creates `linuxcraft/chapter1`, `linuxcraft/chapter2`, and `linuxcraft/chapter3` in a single line.

---

## 8. Creating Files: `touch`

**Definition:** `touch` creates a new, empty file if it doesn't already exist. If the file does already exist, `touch` doesn't erase or change its content, it simply updates the file's last modified timestamp.

**Syntax:**
```bash
touch filename
```

**Example:**
```bash
touch notes.txt
```

**Creating multiple files at once**

**Syntax:**
```bash
touch file1 file2 file3
```

**Example:**
```bash
touch index.html style.css script.js
```

---

## 9. Listing Contents: `ls`

### 9.1 Basic `ls`

**Definition:** `ls` (list) displays the files and directories inside your current location, or a specified path.

**Syntax:**
```bash
ls
```

**Example:**
```bash
ls /home/ibrahim/projects
```

### 9.2 `ls -a`

**Definition:** The `-a` (all) flag reveals hidden files and directories in addition to normal ones. On Linux, any file or directory whose name begins with a dot (`.`) is treated as hidden and skipped by default, commonly used for configuration files.

**Syntax:**
```bash
ls -a
```

**Example:**
```bash
ls -a ~
```
Output includes entries like `.bashrc` and `.ssh`, which a plain `ls` would not show.

### 9.3 `ls -l`

**Definition:** The `-l` (long format) flag displays detailed information about each item: permissions, number of links, owner, group, file size, and last modified date.

**Syntax:**
```bash
ls -l
```

**Example:**
```bash
ls -l projects/
```
Output:
```
drwxr-xr-x 2 ibrahim developers 4096 Aug 20 09:14 linuxcraft
-rw-r--r-- 1 ibrahim developers  512 Aug 21 11:02 notes.txt
```

### 9.4 `ls -h`

**Definition:** The `-h` (human readable) flag converts raw byte counts into more readable units like KB, MB, and GB. On its own it does nothing visible, it's designed to be combined with `-l`.

**Syntax:**
```bash
ls -lh
```

**Example:**
```bash
ls -lh projects/
```
Output:
```
-rw-r--r-- 1 ibrahim developers 2.1M Aug 21 11:02 dataset.csv
```
Without `-h`, that same file would display as `2202009`, a far less readable byte count.

**Combining flags**

**Syntax:**
```bash
ls -lah
```

**Example:**
```bash
ls -lah ~
```
This single command lists everything, including hidden files, in long format, with human readable sizes, arguably the most commonly typed `ls` variant in daily use.

---

## 10. Copying Files and Directories: `cp`

### 10.1 Basic `cp`

**Definition:** `cp` (copy) duplicates a file, creating a second, independent copy at the destination you specify. Changes made to one copy afterward do not affect the other.

**Syntax:**
```bash
cp source destination
```

**Example:**
```bash
cp file1.txt file2.txt
```
This creates `file2.txt` as an exact duplicate of `file1.txt`, both now exist independently.

**Copying a file into a directory**

**Syntax:**
```bash
cp filename directoryname/
```

**Example:**
```bash
cp notes.txt backups/
```

### 10.2 `cp -r`

**Definition:** The `-r` (recursive) flag is required when copying a directory, since `cp` on its own only works on individual files. It copies the target directory along with every file and subdirectory inside it.

**Syntax:**
```bash
cp -r sourcedirectory destinationdirectory
```

**Example:**
```bash
cp -r dir1 dir2
```
This creates `dir2` as a complete copy of `dir1`, including all of its contents.

**Preserving file attributes while copying**

**Definition:** The `-p` flag preserves the original file's permissions, ownership, and timestamps instead of applying new defaults to the copy.

**Syntax:**
```bash
cp -rp sourcedirectory destinationdirectory
```

**Example:**
```bash
cp -rp linuxcraft linuxcraft-backup
```

---

## 11. Moving and Renaming: `mv`

**Definition:** `mv` (move) relocates a file or directory from one location to another. Unlike `cp`, it does not leave a copy behind at the original location, the item is fully transferred.

**Syntax:**
```bash
mv source destination
```

**Example (moving a file into a directory):**
```bash
mv notes.txt archive/
```
This physically moves `notes.txt` into the `archive` directory, it no longer exists at its original location.

### 11.1 Using `mv` to Rename

**Definition:** Linux has no dedicated "rename" command. Because `mv` simply relocates a file to a target path, giving it a new name in the same directory has the practical effect of renaming it.

**Syntax:**
```bash
mv old_name new_name
```

**Example:**
```bash
mv draft.txt final_report.txt
```
`draft.txt` is now called `final_report.txt`, still in the same directory.

**Moving multiple files into one directory at once**

**Syntax:**
```bash
mv file1 file2 file3 destinationdirectory/
```

**Example:**
```bash
mv index.html style.css script.js website/
```

---

## 12. Removing Files and Directories: `rm` and `rmdir`

### 12.1 `rm`

**Definition:** `rm` (remove) permanently deletes a file. There is no recycle bin involved, once a file is removed this way, it is gone.

**Syntax:**
```bash
rm filename
```

**Example:**
```bash
rm file.txt
```

**Removing a directory and everything inside it**

**Definition:** On its own, `rm` refuses to delete directories. Adding the `-r` (recursive) flag allows it to delete a directory along with every file and subdirectory it contains.

**Syntax:**
```bash
rm -r directoryname
```

**Example:**
```bash
rm -r old_project/
```

**Forcing removal without confirmation prompts**

**Definition:** The `-f` (force) flag suppresses confirmation prompts and ignores nonexistent files instead of raising an error. Combined with `-r`, this becomes `rm -rf`, one of the most powerful and most dangerous commands in Linux, since it deletes without asking twice.

**Syntax:**
```bash
rm -rf directoryname
```

**Example:**
```bash
rm -rf temp_files/
```
Use this with real caution, there is no confirmation step and no undo.

### 12.2 `rmdir`

**Definition:** `rmdir` (remove directory) deletes a directory, but only if it is completely empty. If it contains any files or subdirectories, `rmdir` will refuse and return an error, which makes it a much safer choice than `rm -r` when you specifically want to avoid accidentally deleting content.

**Syntax:**
```bash
rmdir directoryname
```

**Example:**
```bash
rmdir empty_folder
```

### 12.3 `rmdir -p`

**Definition:** The `-p` (parents) flag removes a directory and then continues removing each parent directory above it, but only as long as each one is also empty. The moment it reaches a parent that still contains something else, it stops.

**Syntax:**
```bash
rmdir -p path/to/nested/directory
```

**Example:**
```bash
rmdir -p projects/linuxcraft/chapters/chapter2
```
If `chapter2` is empty, it's removed, then if `chapters` is now empty too, it's removed, and so on up the chain, stopping at the first parent that still has other content in it.

### 12.4 A Note on "`rmdir -r`"

It's worth being precise here: `rmdir` does not have a `-r` flag, and was never designed to remove non-empty directories at all, that is intentional, it exists specifically as the safe option. When you need to delete a directory that still has files inside it, the correct command is `rm -r`, covered above, not `rmdir`.

| Command | Deletes Empty Directories | Deletes Non-Empty Directories |
|---|---|---|
| `rmdir` | Yes | No, returns an error |
| `rmdir -p` | Yes, plus empty parents | No |
| `rm -r` | Yes | Yes |

---

## 13. Writing to Files: `echo`

**Definition:** `echo` prints text to the screen. Combined with redirection operators, it can send that text into a file instead of displaying it in the terminal.

**Printing text to the screen**

**Syntax:**
```bash
echo "text"
```

**Example:**
```bash
echo "Hello, LinuxCraft"
```

**Writing text to a file (overwrite)**

**Definition:** The `>` operator redirects output into a file. If the file already exists, its entire previous content is replaced. If it doesn't exist, it's created.

**Syntax:**
```bash
echo "text" > filename
```

**Example:**
```bash
echo "Hello" > file.txt
```
If `file.txt` already had content in it, that content is now gone, replaced entirely by "Hello".

**Appending text to a file**

**Definition:** The `>>` operator also redirects output into a file, but instead of replacing existing content, it adds the new text to the end of the file, leaving everything already there untouched.

**Syntax:**
```bash
echo "text" >> filename
```

**Example:**
```bash
echo "Second line" >> file.txt
```
`file.txt` now contains its original content plus "Second line" added at the end.

---

## 14. Viewing File Content: `cat`

### 14.1 Displaying File Content

**Definition:** `cat` (concatenate) reads a file and prints its entire content to the screen at once, with no scrolling or pagination.

**Syntax:**
```bash
cat filename
```

**Example:**
```bash
cat file.txt
```

**Displaying multiple files at once**

**Syntax:**
```bash
cat file1 file2
```

**Example:**
```bash
cat chapter1.txt chapter2.txt
```
This prints both files' contents one after another, as a single continuous stream of output.

**Displaying content with line numbers**

**Syntax:**
```bash
cat -n filename
```

**Example:**
```bash
cat -n script.sh
```

### 14.2 Using `cat` to Copy Content

**Definition:** Because `cat` outputs a file's content as a stream, that stream can be redirected into a new file, effectively using `cat` as a copy tool. This works identically to `cp` for simple cases, but is especially useful for merging multiple files into one.

**Copying one file into a new file**

**Syntax:**
```bash
cat source > destination
```

**Example:**
```bash
cat original.txt > copy.txt
```
`copy.txt` is created (or overwritten) with the exact contents of `original.txt`.

**Merging multiple files into a single new file**

**Syntax:**
```bash
cat file1 file2 file3 > combined.txt
```

**Example:**
```bash
cat chapter1.txt chapter2.txt chapter3.txt > full_guide.txt
```
`full_guide.txt` now contains the combined content of all three files, in that exact order.

**Appending one file's content onto another**

**Syntax:**
```bash
cat source >> destination
```

**Example:**
```bash
cat notes.txt >> archive.txt
```
The content of `notes.txt` is added to the end of `archive.txt`, without erasing what was already there.

---

## 15. Chapter Summary

By this point you should understand:

* How to gain elevated privileges safely using `sudo`, `su`, and `sudo su`, and the practical difference between them.
* How to confirm your location with `pwd` and return to your home directory instantly.
* The difference between an absolute path and a relative path, and when each one is appropriate.
* How to move through the filesystem with `cd`, including shortcuts like `~`, `..`, and `-`.
* How to identify a file's type directly from a permission string or with the `file` command.
* How to create directories, including nested structures, with `mkdir` and `mkdir -p`.
* How to create empty files or update timestamps with `touch`.
* How to list directory contents with `ls`, and the effect of `-a`, `-l`, and `-h`.
* How to copy files and directories with `cp` and `cp -r`.
* How to move and rename files and directories with `mv`.
* The real difference between `rm -r` and `rmdir`, and why `rmdir -r` does not exist.
* How to write and append text to files using `echo` with `>` and `>>`.
* How to view, combine, and copy file content using `cat`.

---

## 16. Knowledge Check

1. What is the practical difference between using `sudo` and using `sudo su`?
2. Why does `cd projects` sometimes work and sometimes fail, depending on where you run it?
3. What happens if you run `mkdir new/nested/folder` without the `-p` flag, and why?
4. Why does `ls -h` produce no visible change unless combined with `-l`?
5. Why does Linux not have a dedicated `rename` command, and what is used instead?
6. What is the actual difference between `rmdir` and `rm -r`, and why does that distinction matter for safety?
7. What is the difference between `echo "text" > file.txt` and `echo "text" >> file.txt`?

---

## 17. What's Next

Chapter 5 moves into process management, covering how to view, monitor, and control running processes on a Linux system.

---

*This chapter is part of the LinuxCraft series, an open source Linux learning path built for hands on, self paced mastery. Contributions, stars, and forks are welcome.*
