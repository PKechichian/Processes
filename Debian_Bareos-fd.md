# Installation of bareos-fd (Bareos File Daemon)

1. `sudo apt update && sudo apt upgrade`

2. `sudo apt install apt-transport-https gnupg`

3. `wget -qO- https://download.bareos.org/current/Debian_12/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/bareos-keyring.gpg > /dev/null`

4. `echo "deb [arch=amd64] https://download.bareos.org/current/Debian_12/ /" | sudo tee /etc/apt/sources.list.d/bareos.list`

5. `sudo apt update`

6. `sudo apt install bareos-fd`

7. `sudo nano /etc/bareos/bareos-fd.conf`

- Name : nom de ce File Daemon (doit être unique dans votre infrastructure Bareos)
- FDAddress : adresse IP ou nom d'hôte de ce client
- FDport : port d'écoute (par défaut 9102)
- Password : mot de passe pour l'authentification avec le Director

8. `sudo systemctl restart bareos-fd`

9. `sudo systemctl enable bareos-fd`
