## Installation d'Asterisk et configuration de l'utilisateur "Stéphane Groot"

### 1. Installation d'Asterisk

- Suivez ces étapes pour installer Asterisk sur votre système Ubuntu :

   ```bash
   sudo add-apt-repository universe
   sudo apt -y install git curl wget libnewt-dev libssl-dev libncurses5-dev subversion libsqlite3-dev build-essential libjansson-dev libxml2-dev  uuid-dev
   ```

- Télécharger Asterisk depuis le site officiel www.asterisk.org et le décompresser.

- Dans le dossier décompressé, exécuter le sript `/contrib/scripts/get_mp3_source.sh`

- Executer `/contrib/scripts/install_prereq install` pour vérifier la bonne résolution des dépendances

- `./configure`

- `make menuselect`

- Selectionner les 4 fonctionnalités dans le module Add-ons puis `SAVE and EXIT`

- `make`

- `make install`

- `make progdocs`

- `make samples`

- `make config`

- `ldconfig`

- `make basic-pbx`

### 2. Configuration de l'utilisateur

1. **Éditer le fichier `sip.conf`** :

   ```bash
   sudo nano /etc/asterisk/sip.conf
   ```

2. **Ajouter la configuration de l'utilisateur** :

   >[sgroot]
   >type=friend
   >secret=0000
   >fullname=Stéphane Groot
   >mailbox=88012@ff
   >context=Marketing
   >host=dynamic
   >dtmfmode=rfc2833
   >disallow=all
   >allow=ulaw

3. **Éditer le fichier `voicemail.conf`** :

   ```bash
   sudo nano /etc/asterisk/voicemail.conf
   ```

4. **Ajouter la configuration de la messagerie vocale** :

   >88012 => 1234,Stéphane Groot

5. **Redémarrer Asterisk** :

   ```bash
   sudo systemctl restart asterisk
   ```

