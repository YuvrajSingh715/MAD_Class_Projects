# 🚀 Project-01: AWS Web Application Deployment

> **Secure web application deployment using AWS EC2, Apache HTTPD, ALB, Route 53, ACM, Blue/Green, and Canary deployment strategies.**

---

## 📌 Project Overview

This project demonstrates how to deploy web applications on **AWS EC2 using Apache HTTPD** and securely expose them through **HTTPS**.

### 🌐 Applications

* 🏡 **Villa Agency**
* ☕ **Klassy Cafe**

The architecture uses **Route 53, ACM, Application Load Balancer, Target Groups, and EC2** to provide secure and reliable application hosting.

---

## 🛠️ AWS Services & Technologies

| Service / Technology             | Purpose                         |
| -------------------------------- | ------------------------------- |
| 🖥️ **EC2**                      | Hosts the web applications      |
| 🐧 **Apache HTTPD**              | Web server                      |
| ⚖️ **Application Load Balancer** | Distributes incoming traffic    |
| 🎯 **Target Groups**             | Connects ALB with EC2 instances |
| 🌐 **Route 53**                  | DNS management                  |
| 🔐 **AWS ACM**                   | SSL/TLS certificate             |
| 📜 **Shell Script / User Data**  | Automates EC2 configuration     |
| 🔒 **HTTPS**                     | Secures application traffic     |

---

## 🔄 Application Flow

```text
👤 User
   ↓
🌐 Route 53
   ↓
🔐 HTTPS / ACM
   ↓
⚖️ Application Load Balancer
   ↓
🎯 Target Group
   ↓
🖥️ EC2 + Apache
   ↓
🌐 Web Application
```

---

# 🔵🟢 Blue/Green Deployment

### 📖 Definition

**Blue/Green deployment** is a deployment strategy where **two separate environments** are maintained:

* 🔵 **Blue** → Current production version
* 🟢 **Green** → New application version

The new version is deployed and tested in the **Green environment** while users continue accessing the **Blue environment**.

Once the Green environment is verified, traffic is switched from **Blue → Green**.

### 🔄 Flow

```text
🔵 BLUE
Current Production
       │
       │ Users
       ▼
      ALB
       
🟢 GREEN
New Version
       │
       │ Testing
       ▼
   Verification
       │
       ▼
 Switch Traffic
       │
       ▼
🟢 GREEN
New Production
```

### ↩️ Rollback

If the Green version has a problem:

```text
🟢 Green ❌
     ↓
🔵 Blue ✅
     ↓
Restore Traffic
```

### ✅ Advantages

* Minimal downtime
* Easy rollback
* Safe testing of new versions
* Production environment remains unaffected during deployment

---

# 🐤 Canary Deployment

### 📖 Definition

**Canary deployment** is a deployment strategy where a **new application version is initially released to a small percentage of users or traffic**.

The new version is monitored for errors and performance before gradually increasing its traffic.

### 🔄 Example

```text
🔵 Blue   → 90%
🟢 Green  → 10%
```

If everything works correctly:

```text
90% / 10%
    ↓
75% / 25%
    ↓
50% / 50%
    ↓
25% / 75%
    ↓
0% / 100%
```

Eventually, all traffic is sent to the new version.

### ❌ If a problem occurs

```text
🟢 Green → Problem
      ↓
Reduce / Stop Green Traffic
      ↓
🔵 Blue → 100%
```

### ✅ Advantages

* Reduces deployment risk
* Allows real-user testing
* Problems can be detected early
* Easy to stop or reduce traffic
* Safer production releases

---

# 🔵🟢 Blue/Green vs 🐤 Canary

| Feature         | Blue/Green                               | Canary                           |
| --------------- | ---------------------------------------- | -------------------------------- |
| Environments    | Two environments                         | New version gradually introduced |
| Initial traffic | Usually switched between environments    | Small percentage to new version  |
| Testing         | New version tested before traffic switch | Tested with limited real traffic |
| Rollback        | Switch back to Blue                      | Reduce/stop Canary traffic       |
| Main goal       | Fast and clean version switch            | Gradual and controlled release   |

---

## 🏛️ Architecture

```text
                    👤 USERS
                       │
                       ▼
                 🌐 Route 53
                       │
                       ▼
                 🔐 HTTPS / ACM
                       │
                       ▼
              ⚖️ Application Load
                  Balancer (ALB)
                       │
                ┌──────┴──────┐
                │             │
              90%            10%
                │          Canary
                ▼             ▼
           🔵 BLUE         🟢 GREEN
        Stable Version    New Version
                │             │
                ▼             ▼
           🎯 Target        🎯 Target
              Group            Group
                │             │
          ┌─────┴─────┐   ┌───┴────┐
          ▼           ▼   ▼        ▼
        EC2         EC2  EC2      EC2
          │           │   │        │
          └─────┬─────┘   └───┬────┘
                ▼             ▼
             Apache        Apache
                │             │
                └──────┬──────┘
                       ▼
                  🌐 Websites
```

---

## 🔐 HTTPS Architecture

```text
🌐 Domain
    │
    ▼
🌐 Route 53
    │
    ▼
🔐 ACM Certificate
    │
    ▼
⚖️ ALB :443
    │
    ▼
🎯 Target Group
    │
    ▼
🖥️ EC2 :80
    │
    ▼
🐧 Apache
```

**ACM** provides the SSL/TLS certificate, while the **Application Load Balancer** handles HTTPS traffic.

---

## 📦 Application Deployment

EC2 instances are configured using a **Shell Script / User Data**.

The script:

1. Installs Apache HTTPD
2. Downloads the website files
3. Extracts the files
4. Copies them to `/var/www/html`
5. Starts Apache
6. Enables Apache at system startup

This makes the deployment **automated and repeatable**.

---

## 📊 Deployment Flow

```text
🚀 New Application Version
          │
          ▼
     🟢 Green Environment
          │
          ▼
      🧪 Test Version
          │
          ▼
     🐤 Canary Traffic
          │
          ▼
    📊 Monitor Application
          │
      ┌───┴────┐
      │        │
     ❌       ✅
   Problem   Stable
      │        │
      ▼        ▼
🔵 Blue     Increase
100%        Traffic
               │
               ▼
          🟢 Green 100%
```

---

## ✨ Key Benefits

* 🔒 Secure HTTPS communication
* ⚖️ Load-balanced traffic
* 🚀 Automated application deployment
* 🔵🟢 Blue/Green deployment
* 🐤 Canary deployment
* ↩️ Quick rollback
* ⏱️ Minimal downtime
* 🛡️ Reduced deployment risk

---

## 🖼️ Project Diagrams

### Architecture Diagram

<img src="https://github.com/user-attachments/assets/918428c9-66ef-432c-83e9-a5c8aef0599b" alt="AWS Architecture Diagram" width="900"/>

### Deployment Diagram

<img src="https://github.com/user-attachments/assets/bb76051e-80b3-49e7-8423-00070083ffff" alt="AWS Deployment Diagram" width="900"/>

---

## 🎯 Conclusion

This project demonstrates a **secure and reliable AWS web application deployment** using EC2, Apache HTTPD, Application Load Balancer, Route 53, and ACM.

The **Blue/Green deployment model** provides a clean way to switch between application versions with quick rollback, while the **Canary deployment model** allows a new version to be introduced gradually and safely.

---

### 👨‍💻 Learning Focus

**AWS • EC2 • Linux • Apache • ALB • Target Groups • Route 53 • ACM • HTTPS • Blue/Green Deployment • Canary Deployment**
