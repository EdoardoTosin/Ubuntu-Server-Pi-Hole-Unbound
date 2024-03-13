% Ubuntu Server + Pi-Hole + Unbound
% Edoardo Tosin

# Ubuntu Server + Pi-Hole + Unbound

This guide will illustrate how to install Ubuntu Server (operating system) along with the FOSS tools [Pi-Hole](https://en.wikipedia.org/wiki/Pi-hole) (dns ad-blocker) and [Unbound](https://en.wikipedia.org/wiki/Unbound_(DNS_server)) (dns resolver). Furthermore, two-factor authentication (2FA) is used as an additional protection against unauthorized access.

## Requirements

Pi-Hole and Unbound can be installed on an operating system other than Ubuntu Server. In case you use a Raspberry Pi (Model B or Zero) or similar, you can install a different operating system (thus skipping the installation of Ubuntu Server) and follow the rest of the guide. If installed on a computer with Linux OS, 2GB of RAM is also acceptable.

## Ubuntu Server installation preparation

First, proceed with the installation of the Ubuntu Server 22.04 distribution downloadable from the [official site](https://ubuntu.com/download/server).

### Installation on USB drive

After downloading the distribution iso file, it needs to be mounted on a USB drive. To mount the image, you can use Balena Etcher, UNetbootin or Rufus (all FOSS).

### Boot from USB

During the computer's POST phase, press the key to display the boot menu, in order to start the operating system from the USB drive. Select with the arrow keys the choice `Try or Install Ubuntu Server` and press the Enter key.

![Ubuntu Server 1](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_1.jpg)

### Language selection

The first configuration choice concerns the system language. You can select `English` because no graphical interface will be installed on the computer and therefore becomes irrelevant.

![Ubuntu Server 2](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_2.jpg)

### Updating installer

If the version installed on the stick is not the latest, it asks if you want to update (from 22.04 to 22.07) before performing the installation. It is not important because the update will still be done from the command line at the end of the operating system installation. To ignore the update, simply confirm the choice `Continue without updating`.

![Ubuntu Server 3](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_3.jpg)

### Keyboard configuration

Choose the keyboard layout used. Make sure that under `Layout` and `Variant` it says `English` (in my case `Italian` since mine has the italian layout), otherwise choose it from the respective menu. Confirm by pressing the `Ok` choice and then `Done`.

![Ubuntu Server 4](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_4.jpg)

![Ubuntu Server 5](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_5.jpg)

### Installation type

Make sure the box next to `Ubuntu Server` is checked, otherwise select it using the `Space` key. Press enter on `Done` to confirm.

![Ubuntu Server 6](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_6.jpg)

### Network connections

Make sure at least one interface does not say `not connected`, and remember the IP address (which appears after DHCPv4 and without the following slash and number) because it will be needed to connect with the ssh protocol and execute commands remotely. It is important to assign this address as static in the router settings of the network so that it does not change. Press enter on `Done`.

![Ubuntu Server 7](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_7.jpg)

### Proxy configuration

In this case, no proxy is used, so nothing should be entered in the white space but simply confirm again the choice `Done`.

![Ubuntu Server 8](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_8.jpg)

### Alternative repository mirror

Check that there is a mirror to find packages and updates for the operating system. In this case, `http://it.archive.ubuntu.com/ubuntu` is good. Confirm by choosing `Done`.

![Ubuntu Server 9](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_9.jpg)

### Disk space configuration

In the following screen, you can see the formatting of the disk in detail. To confirm, select `Done` and then `Continue`.

![Ubuntu Server 11](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_11.jpg)

![Ubuntu Server 12](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_12.jpg)

### Profile settings

Import the name, server name, username, and password.
It is important not to lose the username and password, otherwise it is no longer possible to log in to the operating system.
The username and password fields will be used later for SSH login.

![Ubuntu Server 13](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_13.jpg)

### SSH settings

Select with the `Space` key the voice `Install OpenSSH server` so that it installs and makes the server accessible via the ssh protocol (default port 22) for remote control.
To confirm, select `Done`.

![Ubuntu Server 14](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_14.jpg)

### Additional components for the server

In this screen, it is possible to enable the installation of additional components to add functionality to the server. In this case, none of them are needed (those that we will install later are not present in this list) so just select `Done` to start the actual installation of the operating system.

![Ubuntu Server 15](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_15.jpg)

## Operating system installation

Now the operating system is installed on the internal hard drive of the computer.

![Ubuntu Server 16](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_16.jpg)

At the end of the procedure, the `Reboot Now` option appears. Press `Enter`.

![Ubuntu Server 17](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_17.jpg)

It will be requested to remove the USB drive that we used for installation. After disconnecting it, press the `Enter` key to reboot the computer.

![Ubuntu Server 18](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_18.jpg)

Now the computer will proceed with the reboot.

![Ubuntu Server 19](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/1_Ubuntu_Server/Ubuntu_Server_19.jpg)

## SSH

After installation, the system and installed packages are updated to their latest versions.

### Login via SSH

To access, open the terminal on another computer in the same network (Powershell in the case of Windows) and type the following command:

```shell
	ssh USERNAME@IP
```
`USERNAME` is replaced with the username entered during the Ubuntu Server installation phase (Profile settings)
Replace `IP` with the IP address of the computer where Ubuntu Server is installed.
`-p 22` is optional since by default ssh connects to port 22 of the device.

![2FA 1](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_1.jpg)

After pressing enter, it will ask to save the SHA256 key. Write `y` and confirm by pressing the `Enter` key.

![2FA 2](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_2.jpg)

Now enter the password set during the Ubuntu Server installation phase (Profile settings).

![2FA 3](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_3.jpg)

If everything has been entered correctly, it is possible to control Ubuntu Server remotely from CLI.

![2FA 4](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_4.jpg)

## System Update

![2FA 5](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_5.jpg)

### Update repositories and system

To start the update, just run the line `sudo apt update && sudo apt upgrade`. Running both commands with `sudo` will ask for the user's password (the same used for login).

![2FA 6](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_6.jpg)

### Confirm updates

To confirm the start of the updates, write `y` and press `Enter`.

![2FA 7](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_7.jpg)

### Restart services

It is likely that a screen will appear asking which services to restart. Select `Ok` without changing anything from the list and then press `Enter` to confirm.

![2FA 8](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_8.jpg)

Now you can restart the computer to verify that with the latest updates the system works before proceeding with the installation of applications.

![2FA 9](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_9.jpg)

## 2FA Installation

In the following procedure, two-factor authentication will be added to reduce the risk of unauthorized access (Zero Trust).

### Package Installation

Log in again following the `Login via SSH` procedure without the paragraph regarding the key.

Execute the command `sudo apt install libpam-google-authenticator` to install the package required for two-factor authentication.

![2FA 10](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_10.jpg)

Enter the user password confirming it. Subsequently, you will be asked to confirm the installation of the package. Write `y` and press `Enter`.

### 2FA Configuration

Now proceed to configure authentication with the temporary 6-digit code (changes every 30 seconds).

![2FA 11](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_11.jpg)

It will be asked whether you want the tokens to be temporary. Write `y` and press `Enter`.

![2FA 12](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_12.jpg)

A secret key (`Your new secret key is:`) will be created for generating time-based codes.
It is important to keep a backup of this key to reduce the risk of losing it.
Scan the QR-Code or insert the key into an application (for Debian distros and derivatives, Authenticator is suitable already present in the system repositories) and save it.
After saving, you can see the code. Insert it into the terminal and press `Enter` to confirm.

![2FA 13](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_13.jpg)

Subsequently, it will be asked whether you want to update the .google_authenticator file. Confirm by writing `y` and pressing `Enter`.

![2FA 14](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_14.jpg)

To increase security, we add restrictions to login every 30 seconds (so only one successful login per each generated code).
To confirm, write `y` and press `Enter`.

![2FA 15](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_15.jpg)

It will be asked whether you want to extend the login with the generated code beyond the time limit from 3 codes to 17 (8 previous, current and 8 subsequent). Since 3 are already enough, it does not need to be extended further so write `n` and press `Enter`.

![2FA 16](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_16.jpg)

Now the addition of the rate-limit will be confirmed, so that only 3 login attempts are possible every 30 seconds. To do this, write `y` and press `Enter`.

![2FA 17](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_17.jpg)

### Adding 2FA to login

To add the just configured two-factor authentication, the common-session file must be modified with the command `sudo nano /etc/pan.d/common-session`. If the password is requested, enter the user password.

![2FA 18](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_18.jpg)

Pressing `Enter` the file to edit will appear. After the line `# end of pam-auth-update config` you must add a new line that contains `auth required pam_google_authenticator.so` as shown in the next screen.
To save the changes and exit the editor, press `CTRL+X` then write `y` and finally press `Enter` to confirm.

![2FA 19](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_19.jpg)

Another file to modify is `sshd_config`. To do this, just open it again with the command `sudo nano /etc/ssh/sshd_config`. If it asks for the password, enter the user password.

![2FA 20](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_20.jpg)

If the `KbdInteractiveAuthentication` option is set to `no`, replace it with `yes`.
To save these changes and exit the editor, press `CTRL+X` then write `y` and finally press `Enter` to confirm.

![2FA 21](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_21.jpg)

Restart the sshd.service service to load the changes with the command `sudo systemctl restart sshd.service`

![2FA 22](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/2_2FA/2FA_22.jpg)

## Pi-Hole Installation

In this phase, Pi-Hole, an open-source software, will be installed. It acts as a [DNS sinkhole](https://it.wikipedia.org/wiki/DNS_sinkhole) and optionally as a DHCP server. This application filters DNS requests to drastically reduce advertising present in web pages.

### Download official repository

As the first step, download the Pi-Hole repository: `git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole`.

![Pi-Hole 2](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_2.jpg)

At the end of the download, enter the folder: `cd 'Pi-hole/automated install/'`.

![Pi-Hole 4](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_4.jpg)

### Prepare Pi-Hole installation

From this directory, launch the bash script: `sudo bash basic-install.sh`.
It may ask for the user password as it is launched with administrator privileges.
If so, enter it and press `Enter`.

![Pi-Hole 6](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_6.jpg)

Wait for a moment for compatibility checks and conflicts.

![Pi-Hole 7](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_7.jpg)

Press `Enter` for each of the two screens to start the configuration.

![Pi-Hole 8](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_8.jpg)

![Pi-Hole 9](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_9.jpg)

### Configure Pi-Hole installation

Since the router has been set as a static IP, we can confirm by selecting `Continue` and pressing `Enter`.

![Pi-Hole 10](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_10.jpg)

Select the same interface to which the IP used throughout the entire procedure was assigned. In this case, the first one. Press `Enter` to confirm.

![Pi-Hole 11](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_11.jpg)

Select a temporary DNS provider. Later it will be removed as the computer with Ubuntu Server will become its own DNS resolver at port `5353` and address `127.0.0.1`.

![Pi-Hole 12](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_12.jpg)

Select `Yes` to include third-party lists to filter content and confirm by pressing `Enter`.

![Pi-Hole 13](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_13.jpg)

Confirm the installation of the web interface for making further changes to the settings. Select `Yes` and press `Enter`.

![Pi-Hole 14](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_14.jpg)

Confirm the installation of the lightweight web server lighttpd to make the web interface functional. Select `Yes` and press `Enter`.

![Pi-Hole 15](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_15.jpg)

Enable logging of queries for potential debugging of problems. Select `Yes` and press `Enter`.

![Pi-Hole 16](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_16.jpg)

In this screen, you can choose how many contents are visible from the admin web interface. Leave the default `Show everything`. Select `Continue` and press `Enter`.

![Pi-Hole 17](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_17.jpg)

Here, the auto-generated password for logging into the web interface via the computer's IP will be displayed. It's not important to remember it since it will be changed with a longer one for security reasons later. Press `Enter` to continue.

![Pi-Hole 18](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_18.jpg)

Once the Pi-Hole configuration procedure is completed, you can proceed to change the admin interface password (not to be confused with the user password).

![Pi-Hole 19](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_19.jpg)

To change the Pi-Hole web interface password, use the command `pihole -a -p`.

![Pi-Hole 20](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_20.jpg)

After pressing `Enter`, enter the password, press `Enter` again, re-enter the password, and finally confirm with the `Enter` key.

![Pi-Hole 21](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_21.jpg)

If the procedure was successful, the text `New password set` will appear.

![Pi-Hole 22](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/3_Pi-Hole/Pi-Hole_22.jpg)

## Unbound Installation

### Package Installation

Now you can proceed with the installation of the DNS resolver Unbound with the command `sudo apt install unbound`.
It may request the user password as it is launched with administrator privileges.
In case affirmative, enter it and press `Enter`.

![Unbound 1](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_1.jpg)

Confirm the application installation by writing `y` and pressing `Enter`.

![Unbound 2](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_2.jpg)

Once the installation is finished, you may see some written messages in red as follows. They can be ignored as they do not affect the functioning of the system and its applications.

![Unbound 3](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_3.jpg)

### Downloading domain registration server resolution IP

Now it is possible to download the list of IPv4 and IPv6 for resolving the domain registration servers. To do this, use the following string: `wget -O root.hints https://www.internic.net/domain/named.root sudo mv root.hints /var/lib/unbound/`.

![Unbound 4](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_4.jpg)

Press `Enter` to start everything.

![Unbound 5](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_5.jpg)

### File Configuration

It is necessary to modify the Unbound configuration file. To do this, write the command `sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf` and confirm with the user password as it is executed with administrator privileges.

![Unbound 6](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_6.jpg)

Within the empty file, the following text should be inserted:

```apacheconf
server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0
    interface: 127.0.0.1
    port: 5353
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    # May be set to yes if you have IPv6 connectivity
    do-ip6: no
    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no
    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"
    # Trust glue only if it is within the server's authority
    harden-glue: yes
    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes
    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no
    # Reduce EDNS reassembly buffer size.
    # Suggested by the unbound man page to reduce fragmentation reassembly problems
    edns-buffer-size: 1472
    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes
    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1
    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m
    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```

Link to the reference file: [`pi-hole.conf`](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/pi-hole.conf)

At lines that begin with `interface` and `port` (in this case containing respectively `127.0.0.1` and `5353`) will have the strings that will subsequently be inserted into the Pi-Hole web interface settings.

![Unbound 7](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_7.jpg)

To exit, press `CTRL+X`. It will ask if you want to save and confirm by writing `y` and pressing `Enter`.

![Unbound 8](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_8.jpg)

Confirm the name and save location of the file by pressing `Enter`.

![Unbound 9](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_9.jpg)

### Check correctness of the file

To be sure that the file has been saved correctly, copy the following command `sudo cat /etc/unbound/unbound.conf.d/pi-hole.conf` and press `Enter` to execute it.

![Unbound 10](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_10.jpg)

Check that it is equal to what was previously copied.

![Unbound 11](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_11.jpg)

### Restart Unbound service

Restart Unbound to load the new configuration: `sudo service unbound restart` and press `Enter`. It may request the user password as it is launched with administrator privileges. In case affirmative, enter it and press `Enter`.

![Unbound 12](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_12.jpg)

### Check DNSSEC operation

The following commands serve to check that DNSSEC works correctly.
First command: `dig pi-hole.net @127.0.0.1 -p 5353`

![Unbound 14](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_14.jpg)

The following command should return SERVFAIL without any IP address: `dig sigfail.verteiltesysteme.net @127.0.0.1 -p 5353`.

![Unbound 16](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_16.jpg)

This command should return NOERROR with an IP address: `dig sigok.verteiltesysteme.net @127.0.0.1 -p 5353`.
If both return correctly then DNSSEC is working.

![Unbound 18](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/4_Unbound/Unbound_18.jpg)

## Pi-Hole Web Interface Configuration

Open the browser and enter the IP of the computer where Ubuntu Server is installed followed by `/admin` as shown in the following figure. When the page loads, click the `Login` item present in the left vertical menu.

![Pi-Hole Web Interface 2](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_2.jpg)

The Pi-Hole web interface password will be asked. Enter it and click `Login` to confirm and enter.

![Pi-Hole Web Interface 3](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_3.jpg)

The following screen is the one before logging in. The difference is that now there is the possibility of configuring Pi-Hole.

![Pi-Hole Web Interface 4](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_4.jpg)

Click the `Settings` item in the left menu.

![Pi-Hole Web Interface 5](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_5.jpg)

Click `DNS` from the items present at the top.

![Pi-Hole Web Interface 6](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_6.jpg)

In the block titled `Upstream DNS Servers` on the left, deselect any tick present. Instead, in the homonymous block on the right, select `Custom 1 (IPv4)` and enter `127.0.0.1#5353` which are the interface and the port that were previously entered in the Unbound configuration file. This will make Pi-Hole ask Unbound for DNS resolution instead of using an external server outside the network.

![Pi-Hole Web Interface 7](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_7.jpg)

In the block titled `Interface settings`, click the `Bind only to interface ...` voice.

![Pi-Hole Web Interface 8](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_8.jpg)

To save all this, scroll down with the mouse and click `Save` located at the bottom right of the screen.

![Pi-Hole Web Interface 9](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_9.jpg)

### Backup Pi-Hole Settings

For safety, download the current Pi-Hole configuration so that in case of need to restore the settings it is faster to do it from a file.
To do this, press click from the horizontal menu present at the top the `Teleporter` voice. Finally, click `Backup` and confirm the file save.

![Pi-Hole Web Interface 10](https://raw.githubusercontent.com/EdoardoTosin/Ubuntu-Server-Pi-Hole-Unbound/main/assets/5_Pi-Hole_Web_Interface/Pi-Hole_Web_Interface_10.jpg)

## Router Configuration

Now from the router settings (refer to the router manual to understand where to find the setting), you can change the DNS to the IP of the computer where Ubuntu Server is installed. This will make every device connected to the network with automatic DNS make a resolution request to the router which in turn will forward the packets to the system filtering most of the advertising from websites and also dangerous content.
