#  **Working with Processes, Daemons, and Signals** 🖥️

Linux is a **multitasking operating system** ⚡. It can run many programs at the same time. Each running program has its own **identity, memory, permissions, and resources** 🛠️. These running programs are called **processes**.

👉 Knowing how processes work and talk to each other is very important for Linux admins and developers.

In this section, you will learn about:

---

## 1️⃣ Introducing Processes

* A **process** = a running program 🏃‍♂️.
* Each process has its own **ID**, **memory**, and **resources** 🔑.
* Linux can run many processes **together** at the same time ⏳.

---

## 2️⃣ Working with Processes

* **Foreground process** 🌟 → runs in front of you (you interact with it directly).
* **Background process** 🌙 → runs silently while you do other tasks.
* Linux tools help you **view 👀, manage ⚙️, and control 🎮** processes.

---

## 3️⃣ Working with Daemons

* A **daemon** 👻 is a special **background process**.
* It usually **starts when the system boots** 🔄 and keeps running in the background.
* Daemons provide services like:

  * 🖨️ Printing
  * ⏰ Scheduling tasks
  * 🌐 Networking

---

## 4️⃣ Exploring Inter-Process Communication

* Processes often need to **communicate 📨**.
* One common method = **signals 🚦**.
* A **signal** = a message sent to a process to:

  * 🛑 Stop it
  * ⏸️ Pause it
  * ▶️ Continue it

---


