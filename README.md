#Bash Scripts

A collection of simple bash scripts that I've created and found useful.  Use the --help flag for a more detailed description of each one.

##beep
Play a sound and show a notification using notify-send.

##ebook-extract
Extract an epub or a mobi ebook into FILENAME.extracted/, or into the specified new target directory.

##encrypted-folder
Create, mount, and unmount encfs volumes.

##external-ip
Display the IP address of this host as seen from the public internet.

##gateway
Display the gateway (typically the router) address of the current network.

##geolocate-ip
Use the ipinfodb.com API to geolocate a list of IP addresses from arguments, a file, or standard input.  By default the API key for ipinfodb.com is read from ~/.config/geolocate-ip/ipinfodb.com-api-key

##kindle-sync
Compile and sync ebooks in ~/documents/ebooks/ to an attached Kindle device, then eject it.  Epub files are compiled to mobi files and cover thumbnails are transferred automatically.

##lock-after-login
Lock the screen, then simulate a mouse movement to immediately wake the screen up.

##nook-sync
Sync ebooks in ~/documents/ebooks/ to an attached Nook device, then eject it.

##pw
Create, edit, and view gpg-encrypted passwords stored in ~/.passwords/.

##remove-old-kernels
Automatically remove unused kernels from the system.

##screen-toggle
Toggle the laptop display on or off.

##set-brightness
Set the laptop screen brightness level.

##socks-tunnel
Control a transparent system-level or browser-level SOCKS tunnel through a remote proxy.

##super-ssh
Open or resume an SSH connection using GNOME Terminal and MOSH.

##todo
Manage the ~/.todo todo file.

##update-dns-record
Check if the IP address of this host matches the DNS records of a remote host.  If it doesn't match, update the DNS records using the Linode API.
