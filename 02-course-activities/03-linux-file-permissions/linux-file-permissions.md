> **Note:** For the best visual formatting and original screenshots, you can download the formal Word document version of this report here: [`File-permissions-in-Linux.docx`](./File-permissions-in-Linux.docx). This Markdown version is provided for quick, convenient in-browser reading. AI was used to convert the original file into markdown format.

---

# File permissions in Linux

## Project description
In this project I am taking the role as a security professional for a research group. I have been tasked with auditing and updating file permissions. The objective is the principle of least privilege for the project files, hidden configuration files and sub-directories. I used learned Linux commands to inspect current file permissions, remove write permissions for unauthorized external users and modified sub-directory permissions to authorized users to prevent unauthorized modification and access to critical files.

---

## Check file and directory details
To begin the audit I used commands to navigate to the working directory to list all files and check permissions.

```text
researcher2@56418b6b645f:~$ cd projects
researcher2@56418b6b645f:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Oct 29 10:29 .
drwxr-xr-x 3 researcher2 research_team 4096 Oct 29 11:18 ..
-rw--w---- 1 researcher2 research_team   46 Oct 29 10:29 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 Oct 29 10:29 drafts
-rw-rw-rw- 1 researcher2 research_team   46 Oct 29 10:29 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Oct 29 10:29 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Oct 29 10:29 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Oct 29 10:29 project_t.txt
```

In this provided output you will see that I used the `cd` command followed by `projects` as I was asked to navigate to the projects folder. When the projects folder was selected I used the command `ls -la` this command tells the terminal to list all the files including hidden files in the folder with permissions displayed.

---

## Describe the permissions string
A permissions string shows the current permissions for a file or directory. The permissions string is 10 characters long. Here is an example of what permissions of a directory could potentially look like: `d-wxr-xrw-`. In my provided example, I will break down what the permissions mean:

* **The first character:** The first character in the permissions string will be either `d` or a hyphen (`-`). This tells whoever is looking at the permission whether what they are looking at is a directory (`d`) or a normal file (`-`).
* **Positions 2–4:** This group of 3 characters represents the user. In this case, the user is `researcher2`, and any permission changes made for these permissions will only affect `researcher2`.
* **Positions 5–7:** This group of 3 characters represents a group. In this case, the group is `research_team`. If any permissions are modified here, they will affect the group, including the user `researcher2`.
* **Positions 8–10:** This group of 3 characters represents "others". Others may be defined as anyone using the same local system who is neither the owner user nor a part of any authorized group.

**What `r`, `w`, and `x` mean:**
* **`r`:** The permission to read and view file contents.
* **`w`:** The permission to write and modify a file.
* **`x`:** The execute permission, which gives permissions for users to run a program or enter a directory.
* **`-`:** Indicates that a specific permission is denied.

---

## Change file permissions

```text
researcher2@aefd4acacf6a:~/projects$ chmod o-w project_k.txt
researcher2@aefd4acacf6a:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 12:32 .
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 13:18 ..
-rw--w---- 1 researcher2 research_team   46 Nov  2 12:32 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 Nov  2 12:32 drafts
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Nov  2 12:32 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_t.txt
researcher2@aefd4acacf6a:~/projects$ chmod g-r project_m.txt
researcher2@aefd4acacf6a:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 12:32 .
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 13:18 ..
-rw--w---- 1 researcher2 research_team   46 Nov  2 12:32 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 Nov  2 12:32 drafts
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_k.txt
-rw------- 1 researcher2 research_team   46 Nov  2 12:32 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_t.txt
```

For this task, I was instructed to check the files in the directory and remove any unauthorized permissions. In the terminal output above, you can see that I used the `chmod` command to update two files. First, I ran `chmod o-w project_k.txt` to remove write permissions (`-w`) from the other category (`o`) on `project_k.txt`. Next, I ran `chmod g-r project_m.txt` to remove read permissions (`-r`) from the group (`g`) so that only the owner can access the restricted `project_m.txt` file. Finally, I used `ls -la` to verify the changes, confirming that `project_k.txt` updated to `-rw-rw-r--` and `project_m.txt` updated to `-rw-------`.

---

## Change file permissions on a hidden file
For this task, I was instructed to check the permissions on a hidden file named `.project_x.txt`. Because files starting with a period are hidden from standard listings, I used `ls -la` to locate it. The scenario states that this file has been archived and should not be written to by anyone, but both the user and the group still need to be able to read it.

Looking at the initial permissions (`-rw---w---`), the user had write permissions that needed to be removed, and the group had write permissions instead of read permissions. In the terminal output below, you can see I used the `chmod` command to fix these permissions all at once: `chmod u-w,g-w,g+r .project_x.txt`. In this command:
* `u-w` removes write permissions from the user.
* `g-w` removes write permissions from the group.
* `g+r` grants read permissions to the group.
* `.project_x.txt` is the hidden target file.

Finally, I ran `ls -la` to confirm the changes took effect. As shown in the terminal output, the permissions for `.project_x.txt` updated to `-r--r-----`, verifying that write access was removed from everyone while keeping read access intact for both the user and the group.

```text
researcher2@aefd4acacf6a:~/projects$ chmod u-w,g-w,g+r .project_x.txt
researcher2@aefd4acacf6a:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 12:32 .
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 13:18 ..
-r--r----- 1 researcher2 research_team   46 Nov  2 12:32 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 Nov  2 12:32 drafts
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_k.txt
-rw------- 1 researcher2 research_team   46 Nov  2 12:32 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 12:32 project_t.txt
```

---

## Change directory permissions

```text
researcher2@da4920af8046:~/projects$ chmod g-x drafts
researcher2@da4920af8046:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 13:41 .
drwxr-xr-x 3 researcher2 research_team 4096 Nov  2 14:18 ..
-rw--w---- 1 researcher2 research_team   46 Nov  2 13:41 .project_x.txt
drwx------ 2 researcher2 research_team 4096 Nov  2 13:41 drafts
-rw-rw-rw- 1 researcher2 research_team   46 Nov  2 13:41 project_k.txt
-rw-r----- 1 researcher2 research_team   46 Nov  2 13:41 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 13:41 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Nov  2 13:41 project_t.txt
```

For the final task, I was instructed to check and modify the permissions on the `drafts` sub-directory located inside the `projects` directory. According to the lab scenario, only the `researcher2` user should be allowed to access the `drafts` directory and its contents, meaning only `researcher2` should have execute privileges. Checking the initial permissions, the directory was set to `drwx--x---`, which allowed members of the `research_team` group to execute and enter the folder. In the terminal output, you can see I ran `chmod g-x drafts` to revoke that access. In this command, `g` targets the group, `-` removes the permission, and `x` targets the execute permission on the `drafts` directory. I then ran `ls -la` to verify the result. The permissions string for `drafts` updated to `drwx------`, confirming that execute access was removed from the group and that only `researcher2` can access the folder.

---

## Summary
In this portfolio activity, I audited and resolved access control issues within the `/home/researcher2/projects` directory using Linux terminal commands. Using `ls -la`, I inspected permissions across visible files, hidden files, and sub-directories to identify security risks that violated the principle of least privilege. I then used the `chmod` command to remove unauthorized write access for others on `project_k.txt`, strip group read permissions on the restricted `project_m.txt`, make the archived hidden file `.project_x.txt` read-only for both user and group, and restrict the `drafts` directory so only the owner has execute privileges. Managing file and directory permissions in this way ensures that sensitive data is protected from unauthorized tampering or exposure across the organization.
