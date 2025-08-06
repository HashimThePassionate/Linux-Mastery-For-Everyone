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
