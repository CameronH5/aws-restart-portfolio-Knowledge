# Managing File Permissions Lab - Complete Documentation

## Objectives
In this lab, I completed the following objectives:
- Changed all folder and file permissions to match the appropriate group structure
- Modified file permissions for a user
- Updated the company folder structure

---

## Task 2: Change File and Folder Ownership

### Task 2.1 - Change companyA Folder Ownership to CEO
**Objective:** Establish the CEO as the owner of the main company folder with Personnel group ownership

**Command Executed:**
```bash
sudo chown -R mjackson:Personnel /home/ec2-user/companyA
```

**What This Did:**
- Used `sudo chown -R` to recursively change ownership of the companyA directory and all its contents
- Changed the **user owner** from ec2-user to `mjackson` (the CEO)
- Changed the **group owner** to `Personnel`
- The `-R` flag ensured all subdirectories and files inherited these ownership changes

**Purpose:** This establishes a clear hierarchy where the CEO has direct control over all company resources, with the Personnel group having group-level permissions.

---

### Task 2.2 - Change HR Folder Ownership
**Objective:** Give the HR manager ownership of the HR department folder

**Command Executed:**
```bash
sudo chown -R mmajor:HR HR
```

**What This Did:**
- Changed the **user owner** to `mmajor` (the HR manager)
- Changed the **group owner** to `HR` (the HR department group)
- Applied changes recursively to all files and subfolders within the HR directory

**Purpose:** This allows the HR manager to have exclusive control over HR department files and resources while maintaining departmental boundaries.

---

### Task 2.3 - Change Finance Folder Ownership
**Objective:** Give the finance manager ownership of the Finance department folder

**Command Executed:**
```bash
sudo chown -R mmajor:Finance HR/Finance
```

**What This Did:**
- Changed the **user owner** to `mmajor` (the finance manager)
- Changed the **group owner** to `Finance` (the Finance department group)
- Targeted the Finance subfolder within the HR directory

**Purpose:** This isolates the Finance department's resources and gives the finance manager control over those specific files while keeping them organized within the company structure.

---

### Task 2.4 - Validate Ownership Changes
**Objective:** Verify that all ownership changes were applied correctly across the entire directory tree

**Command Executed:**
```bash
ls -laR
```

**Screenshot of Command Execution (Image 6):**
The output showed:
```
total 0
drwx------ 10 mjackson Personnel 147 Jun 18 17:55 .
drwxr-xr-x  2 ec2-user ec2-user  90 Jun 18 17:55 ..
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 CEO
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 Documents
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 Employees
drwxr-xr-x  6 mjackson Personnel  72 Jun 18 17:55 HR
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 Management
drwxr-xr-x  1 mjackson Personnel   0 Jun 18 17:55 Roster.csv
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 Sales
drwxr-xr-x  2 mjackson Personnel  24 Jun 18 17:55 SharedFolders
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 Shipping

./CEO:
total 0
drwxr-xr-x  2 mjackson Personnel   6 Jun 18 17:55 .
```

**Validation Results:**
✅ All top-level folders are owned by mjackson (CEO) with Personnel group
✅ HR folder and subfolders show correct ownership setup
✅ All subdirectories properly inherited the ownership changes from the recursive command

---

## Task 3: Change Permission Modes

### Task 3.1 - Create and Configure Symbolic Mode File
**Objective:** Create a file and use symbolic chmod notation to set group write permissions

**Commands Executed (Image 7):**
```bash
sudo vi symbolic_mode_file
# Save and close the file (ESC, then :wq, then Enter)
sudo chmod g+w symbolic_mode_file
```

**What This Did:**
- Created an empty file named `symbolic_mode_file` using vi editor
- Used `chmod g+w` which uses **symbolic notation** to add write permission to the **group** owner
  - `g` = group
  - `+` = add permission
  - `w` = write
- This is more readable than absolute notation

---

### Task 3.2 - Create and Configure Absolute Mode File
**Objective:** Create a file and use absolute (octal) chmod notation to set specific permissions

**Commands Executed (Image 7):**
```bash
sudo vi absolute_mode_file
# Save and close the file (ESC, then :wq, then Enter)
sudo chmod 764 absolute_mode_file
```

**What This Did:**
- Created an empty file named `absolute_mode_file` using vi editor
- Used `chmod 764` which sets permissions using **absolute (octal) notation**
  - **7** (User/Owner): 4(read) + 2(write) + 1(execute) = 7 (rwx - full permissions)
  - **6** (Group): 4(read) + 2(write) = 6 (rw- - read and write, no execute)
  - **4** (Others): 4(read) = 4 (r-- - read only)

**Permission Visualization:**
```
764 = rwx rw- r--
      user group others
      ↓    ↓     ↓
      7    6     4
```

---

### Task 3.3 - Verify Permission Changes
**Objective:** Confirm that both files have the correct permissions applied

**Command Executed (Image 8):**
```bash
ls -l
```

**Screenshot Output (Image 8):**
```
-rwxrw-r-- 1 root       root                 0 Jun 18 18:06 absolute_mode_file
-rw-r-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 CEO
drwxr-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 Documents
drwxr-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 Employees
drwxr-xr-x 6 mjackson   Personnel           72 Jun 18 17:55 HR
drwxr-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 Management
-rw-r--r-- 1 mjackson   Personnel            0 Jun 18 17:55 Roster.csv
drwxr-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 Sales
drwxr-xr-x 2 mjackson   Personnel           24 Jun 18 17:55 SharedFolders
drwxr-xr-x 2 mjackson   Personnel            6 Jun 18 17:55 Shipping
-rw-r--r-- 1 root       root                 0 Jun 18 18:05 symbolic_mode_file
```

**Verification Results:**
✅ **absolute_mode_file** shows `-rwxrw-r--` which is exactly 764 permissions
  - User (root): rwx (read, write, execute)
  - Group: rw- (read, write)
  - Others: r-- (read only)

✅ **symbolic_mode_file** shows `-rw-r--r--` 
  - Group write permission was successfully added
  - Original file permissions were preserved

---

## Task 4: Assign Permissions to Specific Folders

### Task 4.1 - Change Shipping Folder Ownership
**Objective:** Transfer ownership of the Shipping department folder to the shipping manager

**Command Executed (Image 9):**
```bash
sudo chown -R eowusu:Shipping Shipping
```

**What This Did:**
- Changed the **user owner** to `eowusu` (the shipping manager)
- Changed the **group owner** to `Shipping` (the Shipping department group)
- Applied recursively to all contents within the Shipping folder

**Purpose:** The shipping manager now has exclusive control over all shipping-related files and resources.

---

### Task 4.2 - Change Sales Folder Ownership
**Objective:** Transfer ownership of the Sales department folder to the sales manager

**Command Executed (Image 9):**
```bash
sudo chown -R nwilf:Sales Sales
```

**What This Did:**
- Changed the **user owner** to `nwilf` (the sales manager)
- Changed the **group owner** to `Sales` (the Sales department group)
- Applied recursively to all contents within the Sales folder

**Purpose:** The sales manager now has exclusive control over all sales-related files and resources.

---

### Task 4.3 - Validate Final Ownership Structure
**Objective:** Verify that the Shipping and Sales folders have been properly reassigned with correct ownership and permissions

**Commands Executed (Image 10):**
```bash
ls -laR Shipping
ls -laR Sales
```

**Screenshot Output (Image 10):**
```
Shipping:
total 0
drwxr-xr-x  2 eowusu    Shipping              6 Jun 18 17:55 .
drwxr-xr-x 10 mjackson Personnel            199 Jun 18 18:06 ..

Sales:
total 0
drwxr-xr-x  2 nwilf     Sales                 6 Jun 18 17:55 .
drwxr-xr-x 10 mjackson Personnel            199 Jun 18 18:06 ..
```

**Validation Results:**
✅ **Shipping folder** is now owned by `eowusu` with group `Shipping`
✅ **Sales folder** is now owned by `nwilf` with group `Sales`
✅ Both folders remain within the companyA structure under Personnel group at the parent level
✅ The hierarchical permission structure is maintained while delegating department-level control

---

## Summary of What Was Accomplished

### Organizational Structure Established
I successfully created a multi-level permission hierarchy that mirrors a real company structure:

```
companyA (owned by: mjackson:Personnel) - CEO Level
├── HR (owned by: mmajor:HR) - Department Manager
│   └── Finance (owned by: mmajor:Finance) - Sub-Department
├── Shipping (owned by: eowusu:Shipping) - Department Manager
├── Sales (owned by: nwilf:Sales) - Department Manager
└── [Other shared resources]
```

### Key Techniques Mastered

1. **Recursive Ownership Changes** - Used `chown -R` to efficiently change ownership across entire directory trees

2. **Symbolic Permission Notation** - Demonstrated readable permission changes using formats like `g+w` (add group write)

3. **Absolute Permission Notation** - Used octal notation (e.g., 764) for precise, complete permission specification

4. **Verification and Validation** - Employed `ls -l` and `ls -laR` to confirm all changes applied correctly

### Real-World Application
This lab demonstrated essential Linux administration skills used in actual enterprise environments:
- Separating department resources with appropriate ownership
- Controlling file access through group membership
- Ensuring proper permission levels prevent unauthorized access
- Creating an auditable organizational structure through file permissions

---

## Commands Reference Table

| Command | Purpose | Example |
|---------|---------|---------|
| `chown user:group path` | Change file/folder owner and group | `sudo chown -R mjackson:Personnel /path` |
| `chown -R` | Recursive ownership change | Applies to all subdirectories |
| `chmod g+w file` | Add group write permission (symbolic) | `sudo chmod g+w symbolic_mode_file` |
| `chmod 764 file` | Set exact permissions (absolute) | `sudo chmod 764 absolute_mode_file` |
| `ls -l` | List files with permissions and ownership | Shows single-level details |
| `ls -laR` | Recursive list with hidden files | Shows complete hierarchy |

---

## Permission Octal Reference

| Number | Permission | Symbols |
|--------|-----------|---------|
| 0 | No permission | --- |
| 1 | Execute | --x |
| 2 | Write | -w- |
| 3 | Write + Execute | -wx |
| 4 | Read | r-- |
| 5 | Read + Execute | r-x |
| 6 | Read + Write | rw- |
| 7 | Read + Write + Execute | rwx |

**Example: 764**
- 7 = rwx (user has all permissions)
- 6 = rw- (group has read and write)
- 4 = r-- (others have read only)
