# This is a sketchy way of setting up a simple mc server via ansible as a docker container

- This way it should be easy to set up (just run set_up_server.yml on your inventory)
- It lets you upload a existing world as a zip
- to download the world as a zip
- easy whitelist players via cmd on the server -> use whitelist add/remove PLAYERNAME

## If all below is done you should be able to use the folowing cmds

mcserver-terminal -> creates ssh connection 
    whitelist add/remove Playername -> to whitelist players (can be executed on the maschine type -terminal before)
mcserver-update -> essentialy plays the set_up_server.yml again -> can be used to update th mineraft version or to update world
mcserver-download -> downloads the current minecraft map on the specified location as a zip

## you need to create a create a vault and adjust the fields for:

- vault_sudo_root 
- server_IP 
- mc_rcon_password

## Besides that i created some simple bash aliases to easily do stuff on the server:

### To download the world:
<pre>
'''bash
    # Hier ist ein cmd zum backup der Minecraft server welt
    mcserver-download() {
        # --- KONFIGURATION ---
        local SERVER_IP="YOUR_SERVER_IP"
        local USER="" <---- HIER DEN USER DER DAS AUSFÜHRT AUF DEM SERVER
        local KEY_PATH=""  # <--- PFAD ZU DEINEM KEY ANPASSEN
        local REMOTE_DIR="/opt/minecraft/data"
        local BACKUP_BASE="" # <--- PFAD ZU DEINER BACKUP LOCATION
        local TIMESTAMP=$(date +%Y%m%d_%H%M)
        local ZIP_NAME="world_$TIMESTAMP.zip"

        # Lokalen Ordner erstellen
        mkdir -p "$BACKUP_BASE"

        echo ">>> 1. Speichervorgänge pausieren (RCON)..."
        ssh -i "$KEY_PATH" $USER@$SERVER_IP "sudo docker exec mc_server rcon-cli save-off && sudo docker exec mc_server rcon-cli save-all"

        echo ">>> 2. Welt auf dem Server zippen (bitte warten)..."
        # Wir zippen den Inhalt von /data direkt nach /tmp
        ssh -i "$KEY_PATH" $USER@$SERVER_IP "cd $REMOTE_DIR && sudo zip -r /tmp/$ZIP_NAME ."

        echo ">>> 3. Speichervorgänge wieder aktivieren..."
        ssh -i "$KEY_PATH" $USER@$SERVER_IP "sudo docker exec mc_server rcon-cli save-on"

        echo ">>> 4. Download der ZIP-Datei..."
        scp -i "$KEY_PATH" $USER@$SERVER_IP:/tmp/$ZIP_NAME "$BACKUP_BASE/"

        echo ">>> 5. Aufräumen (temporäre ZIP auf Server löschen)..."
        ssh -i "$KEY_PATH" $USER@$SERVER_IP "sudo rm /tmp/$ZIP_NAME"

        echo "---------------------------------------------------------------"
        echo "FERTIG! Welt als ZIP gesichert: $BACKUP_BASE/$ZIP_NAME"
    }
</pre>

### To quickly get in the terminal of the server
<pre>
alias mcserver-terminal="ssh YOURUSER@SERVER_IP -i HIER_PFAD_KEY"
</pre>

### To update the server
<pre>
alias mcserver-update="cd Nextcloud/Privat/Code/minecraft-ansible;ansible-playbook set_up_server.yml --ask-vault-pass"
</pre>
