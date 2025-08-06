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

