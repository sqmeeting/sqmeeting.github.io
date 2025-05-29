# Configuration

This page explains how to configure SQMeeting Server after installation to provide basic meeting services.

To configure the following functions, you need to log in to the SQMeeting Server web management page:

Login address: https://server-IP:7443, e.g.: https://10.150.1.202:7443

Default admin user: admin    
Password: Admin123!@#  (Please change the password after login)

## 1. System Settings

After logging into the web management page, navigate to System Management -> System Settings to configure the network and ports required by the system.

- If your system only works on the intranet, you can keep the default settings. You can also configure the intranet domain name and management port (default: 7443) as needed; in the System Settings → Node List → Modify page, configure the media ports (default: 32500-32507).

- If you deploy the SQMeeting Server in an enterprise DMZ or cloud platform and need to access SQMeeting Server's login, scheduling, and meeting services from the public network, you need to configure the following:

- ✅Preparation: Public IP, domain name (optional).

- ✅Configure firewall: Map the public IP (e.g., 36.105.101.82) to the internal IP of SQMeeting Server (e.g., 10.150.1.202) on the DMZ or cloud platform firewall, and open the management port and media port group configured in your system (e.g., open tcp: 7443, udp: 32500 - 32507).

- ✅Configure public network: Navigate to SQMeeting Server management page → System Management → System Settings, configure the public management IP (icon 1), management port, public media IP (icon 2) and media ports, submit the changes, and the service will automatically restart for the changes to take effect.

![System Configuration](./images/config_basic.jpg)

After configuration is complete, you can access SQMeeting services through the domain name or public IP address.

## 2. NTP Settings

System Management -> NTP Settings, configure NTP server to ensure system time synchronizes with standard time, otherwise the service cannot operate normally.

![NTP Configuration](./images/config_ntp.jpg)

## 3. Software License

Check the software license to ensure it is still valid.

![License Configuration](./images/config_license.jpg)

Server installation and basic configuration is now complete


## 4. Certificate

The system comes with a default self-signed certificate. When accessing the server website through a browser, you may receive security warning prompts.

It is recommended that users install a certificate authenticated by a CA authority. You can upload your own certificate through the certificate management interface on the server website. Please refer to the image below for certificate format requirements:

![Certificate Configuration](./images/config_certificate.jpg)


## 5. License

How to install a license:

![License Installation](./images/config_license_serverid.jpg)

1. Get the Server ID: Navigate to System Management -> Software License, click Add License, then click Copy to get the Server ID.
   Note: For HA license applications, you need to obtain Server IDs from both the HA primary server and HA secondary server.

2. Apply for a license: Send the Server ID to the community administrator, specify the license type, purpose, and quantity, then wait for the community administrator's approval.

3. Install the license: Navigate to System Management -> Software License, click Add License, paste the approved license into the License Key field, and click Submit.
   Note: To install an HA license, you need to log in to the web management page of the HA primary server.

4. Confirm the license: Check the installed license on the license page to ensure the license type, quantity, validity period, etc., meet your requirements.


![License Installation](./images/config_license.jpg)

## 6. System Upgrade

How to perform a system upgrade:

1. Download the SQMeeting system upgrade package:
   - For x86 platform: FrtcServer-x86_64-x.x.x-xxxxx.bin
   - For arm64 platform: FrtcServer-arm_64-x.x.x-xxxxx.bin

2. Navigate to SQMeeting Server management page → System Management → System Upgrade, upload the upgrade package to perform the upgrade. The upgrade process takes approximately 10 minutes.

### 7. System Rollback

The SQMeeting system does not support automatic rollback. If you have installed a higher version and want to roll back to a lower version, you need to first remove the existing system and then reinstall it.

### 8. How to Remove the Existing System

Use the frtc-setup remove tool in the installation directory to uninstall the server platform software system, and restart the server after completion.

```bash
./frtc-setup remove
```

In the interactive environment, enter "yes" to keep backup data; enter "no" to not keep backup data.

After execution is complete, the system will automatically restart.

### 9. How to Open the Service Platform Console

SSH login to the server, run the command `su - frtc_console` to open the console for configuration and diagnostics.

```bash
su - frtc_console
```

### 10. How to View or Modify the Service Platform's Network/DNS/Hostname

1. Launch the Service Platform Console

   After logging in with the system root account or other accounts, execute the command `su - frtc_console`

2. Network Settings

   After the service platform is installed, it uses DHCP by default. Please confirm that the address obtained by DHCP is consistent with the IP address configured in the operating system. Use this address to directly access the SQMeeting Server Web page for the next step of system configuration and management.

![Network Configuration](./images/config_dhcp.png)

   You can also choose the Static mode to set up the system network. In the Boot Proto option, select Static, configure the static IP address (consistent with the operating system address), subnet mask, gateway, and DNS server. After setting, click Apply. After confirming the submission as prompted, the system will automatically restart. Once the restart is complete, the network settings will take effect.

![Network Configuration](./images/config_static.png)

3. Setting Hostname and Domain

   Configure Hostname and Domain, click Apply to save and make the settings take effect.

### 11. How to Enhance System Security

You can enhance system security through the following methods:

1. Disable SSH Service
   
   After the service platform is installed and deployed, you can disable the SSH service to improve system security. If you perform a system security vulnerability scan, the SSH service must be turned off before scanning.
   
   To disable SSH, open the server console, navigate to the Diag page, select "Disable SSH", press Enter to confirm, and then click OK to confirm.

2. System Security Settings
   
   Access SQMeeting Server web page → System Management → Security Settings, set "Password Security Level" and "Password Validity Period", enable "Prohibit External Network Access to Management Page" to enhance system security.


### 12. Test Basic Functions

Check the software license to ensure it is still valid.

1. Schedule a meeting on the web management page, check the meeting details, and get the meeting URL or QR code.
2. Install two SQMeeting clients, join the meeting by clicking the URL or scanning the QR code, and test if audio and video are working normally.
3. After the basic test is passed, you can start using SQMeeting for meetings and other audio-video collaboration.

