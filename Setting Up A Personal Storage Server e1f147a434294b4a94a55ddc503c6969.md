# Setting Up A Personal Storage Server
## About
For my project, I've decided to learn, setup, and share about my experience setting up a personal storage server for myself. I don't really feel like pay $10 a month for something I could easily do myself. Hope you enjoy! :) 

## What Is Next Cloud?

NextCloud is an open source file sharing service that let’s you store many of the things that you might store on platforms like Google Drive or DropBox.

## Step 1: Ubuntu

Regardless of the device that you use, you’re going to want to download Ubuntu onto it to run the server. Most of the world’s backend computers run on linux, so this should come as no surprise.

**Step1a.) Download an Ubuntu Image**

**Step2a.) Download the iso onto a USB stick using an etcher like balenaEtcher.**

**Step3a.) Boot from flash drive using F12 on bootup to activate your motherboard’s boot system menu**

**Step4a.) Follow Installation Setup**

**Ubuntu has a great installation guide that you can follow here:**

[https://ubuntu.com/tutorials/install-ubuntu-desktop#2-download-an-ubuntu-im](https://ubuntu.com/tutorials/install-ubuntu-desktop#2-download-an-ubuntu-image)

## Step 2: Install NextCloud

************Snap************ is Ubuntu’s default package manager. We’re going to use that package manager to install NextCloud

```jsx
sudo snap install nextcloud
```

Confirm it installed using

```jsx
snap changes nextcloud
```

Okay awesome, next we want to create an admin account that can allow us to configure the server in a way that works for us.

```jsx
sudo nextcloud.manual-install [INSERT_USERNAME_HERE] [INSERT_PASSWORD_HERE]
```

Great now we have successfully installed NextCloud with a admin!

## Step 3: Domain Setup

1. **Choose a Domain Service**
2. **Provide Contact Information**
3. **Configure Domain Settings:**
    - After registration, log in to your domain registrar's control panel. Navigate to the domain management section where you can configure various settings.

**Configuring DNS Settings:**

1. **Access DNS Management:**
    - Look for a section in your domain registrar's control panel related to DNS management or domain settings. It may be called "DNS Management," "Name Servers," or something similar.
2. **Set Name Servers:**
    - You'll typically need to set the domain's name servers to point to those provided by your hosting provider or domain registrar.
    - Use this command to get your public ip address
    - `ip a`
3. **Create DNS Records:**
    - Configure DNS records to direct traffic to your server's public IP address. The two main types of records you'll likely need are:
        - **A Record (Address Record):** Associates your domain with an IP address. Create an A record pointing to your server's public IP.
        - **CNAME Record (Canonical Name):** Optionally used for subdomains. For example, you might create a CNAME record for **`www`** to point to your domain.
    
    Example:
    
    ```css
    cssCopy code
    Type    Name        Value
    ----    ----        -----
    A       @           Your_Server_Public_IP
    CNAME   www         your-domain.com
    
    ```
    
    Go back to your Ubuntu server and add your domain:
    
    ```jsx
    sudo nextcloud.occ config:system:set trusted_domains 1 --value=YOUR_DOMAIN
    ```
    
    Verify [localhost](http://localhost) and your domain are the two trusted domains
    
    ```jsx
    sudo nextcloud.occ config:system:get trusted_domains
    ```
    
    ## Step 3: Setup HTTPS for Web Portal
    
    Allow incoming traffic on certain ports
    
    ```jsx
    sudo ufw allow 80,443/tcp
    ```
    
    Install Let’s Encrypt:
    
    ```jsx
    sudo nextcloud.enable-https lets-encrypt
    ```
    
    Step 3a.) Follow setup instructions
    

## Step 4: Test And Login

4a.) Go to browser.

4b.) Type in domain name and hit enter.

4c.) If you quickly completed this tutorial, allow time for the DNS servers to propagate and point to your domain name this may take a few minutes to a day, but tends to only take 10 minutes or so.