# Guacamole 1.5.3 VDI/Jump Server Appliance Build Script

<img src="https://github.githubassets.com/images/icons/emoji/sparkles.png" width="35"> This suite of build scripts makes setting up Guacamole a breeze. Its got installer support for TLS reverse proxy, Active Directory integration, multi-factor authentication, Quick Connect & History Recording Storage UI enhancements, dark mode and custom UI templates, auto database backup, O365 email alerts, internal daemon security hardening options and even a fail2ban policy for defence against brute force attacks. There's also code in here to get you up and running with an enterprise deployment approach near identical to [Amazon's Guacamole Bastion Cluster](http://netcubed-ami.s3-website-us-east-1.amazonaws.com/guaws/v2.3.1/cluster/), if that's your thing!

## Automatic Installation

<img src="https://github.githubassets.com/images/icons/emoji/rocket.png" width="35"> To start building your Guacamole appliance, paste the below link into a terminal and just follow the prompts **(no need for sudo, but the user must be a member of the sudo group)**:

```shell
wget https://raw.githubusercontent.com/itiligent/Guacamole-Install/main/1-setup.sh && chmod +x 1-setup.sh && ./1-setup.sh
```

## Prerequisites (Yes! Debian 12 is now supported!)

<img src="https://github.githubassets.com/images/icons/emoji/lock.png" width="35"> **Before diving in, make sure you have:**

- **A compatible OS:**
    - **Debian 12, 11 or 10**
    - **Ubuntu 23.04, 22.04, 20.04 & 18.04**
    - **Raspbian Buster & Bullseye**
    - **Official vendor cloud images equivalent to the above.**
- Minimum 8GB RAM and 40GB HDD.
- DNS entries matching your default appliance network interface IP (essential for TLS).
- Open TCP ports: 22, 80, and 443.

## Installation Menu

<img src="https://github.githubassets.com/images/icons/emoji/wrench.png" width="35"> **The main script guides you through the installation process in the following steps:**

1. Confirm your system hostname and local DNS domain suffix. (Must be consistent for TLS proxy)
2. Choose a locally installed or remote MySQL instance, set database security preferences.
3. Pick an authentication extension: DUO, TOTP, LDAP, or none.
4. Select optional console features: Quick Connect & History Recorded Storage UI integrations.
5. Decide on the Guacamole front end: Nginx reverse proxy (http or https) or keep the native Guacamole interface
  - If you opt to install Nginx with self signed TLS:
    - New server and client browser certificates are backed up to `$HOME/guac-setup/tls-certs/date-time`
    - Pay attention to on-screen instructions for client certificate import (no more pesky browser warnings). 

## Managing self signed TLS certs with Nginx (the easy way!)

   - **To renew certificates, or to change the reverse proxy local dns name and/or IP address:** 
     - Just re-run ` 4a-install-tls-self-signed-nginx.sh`as many times as you like (accompanying server and browser client certs will also be updated). Look in the script comments for command line options.
   

## Active Directory Integration

<img src="https://github.githubassets.com/images/icons/emoji/key.png" width="35"> **Need help with Active Directory authentication?** Check [here](https://github.com/itiligent/Guacamole-Install/blob/main/ACTIVE-DIRECTORY-HOW-TO.md).

## Customise & Brand Your Guacamole Theme

<img src="https://github.githubassets.com/images/icons/emoji/art.png" width="35"> **Want to give Guacamole your personal touch? Follow the theme and branding instructions** [here](https://github.com/itiligent/Guacamole-Install/tree/main/custom-theme-builder).

## Installation Instructions

<img src="https://github.githubassets.com/images/icons/emoji/unicode/2699.png" width="35"> 

### **Paste and the wget autorun link, thats it! *But if* you want to make Guacamole your own and customise...**
**Exit `1-setup.sh` at the first prompt**. All the configurable options can be found at the start of `1-setup.sh`. Certain combinations of edits will even produce an unattended install!

**Other useful install notes:**
- **Caution: Be aware that running the auto-run link again re-downloads the suite of scripts and will overwrite your changes. You must run setup locally after editing the setup script.** (Also be sure to comment out the download links in the setup script for any other scripts you want to edit, but there should be little need to touch these.)
- Many of the scripts in the suite are **automatically adjusted with your chosen installation settings** to form a matched & transportable set. This allows you to add extra features after installation whilst avoiding mismatches with the original install.
- Nginx is automatically configured to use TLS 1.2 or above (so really old browser versions may not work.)
- A daily MySQL backup job will be automatically configured under the script owner's crontab.
- **Security info:** The Quick Connect and History Recorded Storage options bring a few security implications; so be aware of potential risks in your particular environment.

**For the more security minded, there's several post-install hardening script options available:**

- `add-fail2ban.sh`: Adds a lockdown policy for Guacamole to guard against brute force attacks.
- `add-tls-guac-daemon.sh`: Wraps internal server daemon <--> guac application traffic in TLS.
- `add-auth-ldap.sh`: A template script for Active Directory integration.
- `add-smtp-relay-o365.sh`: A template script for email alerts integrated with MSO65 (BYO app password).


## Upgrading Guacamole

<img src="https://github.githubassets.com/images/icons/emoji/globe_with_meridians.png" width="35"> To upgrade Guacamole, edit `upgrade-guac.sh` to relfect the latest versions of Guacamole and MySQL connector/J before running it. This script will also automatically update the installed extensions.

## Enterprise Scale Out & High Availability 

<img src="https://github.githubassets.com/images/icons/emoji/unicode/1f454.png" width="35"> For Enterprise deployments, did you know that Guacamole can be run in a load balanced farm? To achieve this, the database, application and front end components are usually **split into 2 or 3 layers.** (VLANs & firewalls between the layers helps with security too.) See [here](https://github.com/itiligent/Guacamole-Install/tree/main/guac-enterprise-build) for how to get started.

Even for home lab use, a simple benefit of using a separate MySQL backend means you can test and break things with your Guacamole server whilst keeping all your connection profiles intact. Just rebuild a fresh server and immediately all your connection profiles and settings are right there!

- **For the DATABASE layer:** Find the included `install-mysql-backend-only.sh` to install a standalone instance of the Guacamole MySQL database for your backend.
- **For the APPLICATION layer:** Simply use the main setup script to build as many application servers as you like, just use the installer to point these to the backend database, making sure to **say no to both the "Install MySQL locally" option and any Nginx install options**.
- **For the Front end**: There are so many choices available that are already very well documented. You could even take the (portable) Nginx scripts to build a separate TLS front end layer. Be aware that [HA Proxy](https://www.haproxy.org/) generally provides far superior session affinity and persistence under load balanced conditions [when compared to Open Source Nginx](https://www.nginx.com/products/nginx/compare-models/) as only Nginx Plus subscribers get all the proper load balancing stuff!)


### Installer script download manifest



<img src="https://github.githubassets.com/images/icons/emoji/package.png" width="35"> The autorun link downloads these repo files into `$HOME/guac-setup`:

For a level of Independence away from the main installer (and flexible server life-cycles), most of the below scripts can be run to include extra features not added at original install (where noted).

- `1-setup.sh`: The installation script.
- `2-install-guacamole.sh`: Guacamole main source build installation script.
- `3-install-nginx.sh`: Installs Nginx for reverse proxy if not added at install.
- `4a-install-tls-self-signed-nginx.sh`: Configures or updates self-signed TLS for Nginx if not added at install.
- `4b-install-tls-letsencrypt-nginx.sh`: Installs Let's Encrypt for Nginx if not added at install
- `add-auth-duo.sh`: Installs Duo MFA extension if not added at install.
- `add-auth-ldap.sh`: Installs Active Directory extension if not added at install.
- `add-auth-totp.sh`: Adds TOTP MFA extension if not added at install.
- `add-xtra-quickconnect.sh`: Adds Quick Connect console extension if not added at install.
- `add-xtra-histrecstore.sh`: Adds History Recorded Storage extension if not added at install.
- `add-smtp-relay-o365.sh`: Sets up SMTP auth relay with O365 for backup messages, monitoring & alerts (BYO app password).
- `add-tls-guac-daemon.sh`: Adds TLS wrapper for guacd server daemon to Guacamole client app internal traffic.
- `add-fail2ban.sh`: Adds a fail2ban policy for brute force attack protection.
- `backup-guacamole.sh`: A MySQL Guacamole backup script.
- `upgrade-guac.sh`: Upgrades Guacamole, all installed extensions and the MySQL connector.
- `branding.jar`: An example template for customising Guacamole's UI theme. Delete to keep the default UI.

Happy Guacamole-ing! 😄🥑
