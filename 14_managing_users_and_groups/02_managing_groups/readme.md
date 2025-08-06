#  **Managing Groups** 🐧

## 📌 Introduction to Linux Groups

In Linux, **groups** are used to organize users who share common attributes or permissions. Each group is uniquely identified by a Group ID (**GID**). Users within the same group share the same GID.

### 📂 Types of Groups

From a user's perspective, there are two types of groups:

* **Primary Group**:

  * User’s default login group.

* **Supplementary (Secondary) Groups**:

  * Additional groups that a user can be a part of.
  * Users can belong to multiple supplementary groups or none.

**Example**: Developers might have a shared `developers` group for accessing specific resources.

---

## 🚧 Managing Linux Groups

### 🔨 Creating a Group

To create a group, use the command `groupadd`.

#### Basic Syntax:

```bash
sudo groupadd [OPTIONS] GROUP_NAME
```

#### Example:

Creating a group named `developers` with default settings:

```bash
sudo groupadd developers
```

#### Checking Group Information:

Group information is stored in `/etc/group`.

To verify group creation:

```bash
cat /etc/group | grep developers
```

#### Output Explanation:

Each entry in `/etc/group` is colon-separated (`:`):

```
developers:x:1003:
```

* **developers**: Group name.
* **x**: Indicates password encrypted (hash stored in `/etc/gshadow`).
* **1003**: GID (unique identifier).

Or using the `getent` command:

```bash
getent group developers
```

### 🎯 Creating a Group with Specific GID

Specify a custom GID with `-g`:

```bash
sudo groupadd -g 1200 developers
```

### 📖 Documentation:

For more details:

```bash
man groupadd
```

---

## 📁 Files for Group Information

* **`/etc/group`**: Generic group membership details.
* **`/etc/gshadow`**: Encrypted password hashes for groups.

**Example entries:**

`/etc/group`:

```bash
ubuntu:x:1000:
alex:x:1001:
developers:x:1002:
developer:x:1200:
```

`/etc/gshadow`:

```bash
ubuntu:!::
alex:!::
developers:!::
```

---

## ⚙️ Commands Explained with Outputs

### Command:

```bash
getent group users
```

#### Output:

```bash
users:x:100:alex
```

* **users**: Group name
* **x**: Encrypted password placeholder
* **100**: GID
* **alex**: Member of the group

### Verifying Group Entries:

#### Checking last 3 groups in `/etc/group`:

```bash
tail -n 3 /etc/group
```

#### Example output:

```bash
alex:x:1001:
developers:x:1002:
developer:x:1200:
```

#### Checking last 3 groups in `/etc/gshadow`:

```bash
tail -n 3 /etc/gshadow
```

#### Example output:

```bash
ubuntu:!::
alex:!::
developers:!::
```

---

### 🔐 **Understanding Group Passwords**

By default, a newly created group does not have a password:

```bash
sudo groupadd developers
```

To securely set a password for a group, use the `gpasswd` utility:

```bash
sudo gpasswd developers
```

You will see a prompt to set and confirm the password:

```bash
Changing the password for group developers
New Password:
Re-enter new password:
```

**Purpose**: Group passwords control access to resources. Group members aren't prompted for a password upon login (`newgrp`), but non-members will need the group password.

To remove a group's password, use:

```bash
sudo gpasswd -r developers
```

---

## ⚙️ **Modifying Groups**

Use the `groupmod` command to modify groups:

**Change Group ID (GID)**:

```bash
sudo groupmod -g 1201 developers
getent group developers
```

Output:

```bash
developers:x:1201:
```

**Rename a Group**:

```bash
sudo groupmod -n devops developers
getent group devops
```

Output:

```bash
devops:x:1201:
```

**Change Group Password**:

```bash
sudo gpasswd devops
```

To remove the password:

```bash
sudo gpasswd -r devops
```

---

## 🗑️ **Deleting Groups**

Use the `groupdel` command:

```bash
sudo groupdel devops
```

If the group is a primary group for a user, it can't be deleted directly. Change the user's primary group first:

```bash
id alex
sudo usermod -g alex alex
id alex
sudo groupdel devops
```

---

## ✏️ **Modifying Groups via `/etc/group`**

It's safer to edit groups using `vigr`:

```bash
sudo vigr
```

After editing:

```bash
developers:x:1200:alex
```

To edit `/etc/gshadow`:

```bash
sudo vigr -s
```

---

## 👥 **Managing Users in Groups**

### ✅ **Adding Users to Groups**

Create groups first:

```bash
sudo groupadd -g 1100 admin
sudo groupadd -g 1200 developer
sudo groupadd -g 1300 devops
```

Add users:

```bash
sudo useradd -g admin -G developers,devops alex2
sudo useradd -g admin -G developers,devops julian2
```

Check group memberships:

```bash
id alex2
id julian2
```

### 🚚 **Moving and Removing Users Across Groups**

Create a new group and add a user:

```bash
sudo groupadd -g 1400 managers
sudo usermod -a -G managers alex2
id alex2
```

To explicitly set secondary groups:

```bash
sudo usermod -G developers,devops,managers alex2
```

To remove secondary groups and retain only one:

```bash
sudo usermod -G managers alex2
id alex2
```

Remove all secondary groups:

```bash
sudo usermod -G '' alex2
id alex2
```

Change primary group:

```bash
sudo usermod -g managers alex2
id alex2
```

Assign an exclusive primary group matching the user's UID:

```bash
sudo groupadd -g 1004 alex2
sudo usermod -g alex2 alex2
id alex2
```

---

#  **Viewing Groups** 📘

## 📂 Group Information Files

Group-related data is stored in two main files:

* `/etc/group`: Contains group names, GIDs, and members.
* `/etc/gshadow`: Stores secure group information (like passwords).

We’ll mostly work with `/etc/group` for our needs.

## 🔍 View All Groups

### ✅ Step 1: Update and Install Utilities

```bash
apt update
apt install -y bsdmainutils
apt install less
```

These commands:

* `apt update`: Refreshes the package list.
* `apt install -y bsdmainutils`: Installs utilities including `column`.
* `apt install less`: Installs a pager for scrolling long output.

### 🧾 Step 2: List All Group Names

```bash
cat /etc/group | cut -d: -f1 | column | less
```

**Explanation:**

| Command Part     | Description                                                 |
| ---------------- | ----------------------------------------------------------- |
| `cat /etc/group` | Outputs the group file                                      |
| `cut -d: -f1`    | Extracts first field (group names) using ":" as a delimiter |
| `column`         | Formats the list into multiple columns                      |
| `less`           | Opens the output in scrollable viewer                       |

**Sample Output:**

```
root     daemon   bin     sys    adm     tty     disk     lp     mail     news
...
alex     developers  devops  managers
```

## 📋 Using `getent` to View Groups

```bash
getent group | cut -d: -f1 | column | less
```

* `getent group`: Retrieves group entries using system database.
* Output is identical to using `/etc/group`.

### 🔍 View Specific Group Details

```bash
getent group developers
```

**Output:**

```
developers:x:1201:julian2,alex2,alex
```

Shows:

* Group name: `developers`
* GID: `1201`
* Members: `julian2, alex2, alex`

## 👤 View a User's Group Memberships

### 🔎 Groups of a Specific User

```bash
groups alex2
```

**Output:**

```
alex2 : alex2 developers devops managers
```

* The first group is the **primary group**.
* The rest are **secondary (supplementary) groups**.

### 🙋‍♂️ Groups of Current User

```bash
groups
```

**Output:**

```
alex developers devops managers
```

No username = your own groups.

---

## 🔄 Group Login Sessions

### 🧠 Concept

When a user logs in:

* The **primary group** is automatically active.
* User-initiated tasks (e.g., creating a file) are associated with this group.

## 🔑 Switching Group Sessions: `newgrp`

### 🔁 Syntax

```bash
newgrp GROUP
```

Switches the current session to a different group (if the user is a member).

## 🧪 Example: Simulating alex2

### 🔧 Step 1: Check `alex2`'s Group Info

```bash
id alex2
```

**Output:**

```
uid=1002(alex2) gid=1004(alex2) groups=1004(alex2),1201(developers),1300(devops),1400(managers)
```

### 👤 Step 2: Switch to User `alex2`

```bash
su alex2
```

* Requires alex2’s password.

### ✅ Confirm Identity

```bash
whoami
```

**Output:**

```
alex2
```

```bash
groups
```

**Output:**

```
alex2 developers devops managers
```

### 📋 Show All Group IDs and Names

```bash
id
```

**Output:**

```
uid=1001(alex) gid=1001(alex) groups=1001(alex),1201(developers),1300(devops),1400(managers)
```

### 🔍 Show Current Primary Group ID

```bash
id -g
```

**Output:**

```
1001
```

Represents the **GID** of the **primary group**.

### 🔄 Change to Group `developers`

```bash
newgrp developers
```

```bash
id -g
```

**Output:**

```
1201
```

This confirms the group session is now **developers**.

## 🚫 Accessing Unauthorized Groups

If a user tries to switch to a group they’re **not a member of**, like:

```bash
newgrp managers
```

They’ll be **prompted for a password** for that group.

If the user knows the group’s password or is a **superuser**, access is granted. Otherwise, it's denied.

---
