###Pre-Requis

Installation du Daemon Ptero 

    apt -y install nodejs git unzip zip make gcc g++ curl sudo
    curl -sSL https://get.docker.com/ | CHANNEL=stable bash
    systemctl enable docker
    nano /etc/default/grub
     - GRUB_CMDLINE_LINUX_DEFAULT="swapaccount=1"
     - Reboot
    
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    mkdir -p /srv/daemon /srv/daemon-data
    cd /srv/daemon
    curl -L https://github.com/pterodactyl/daemon/releases/download/v0.6.12/daemon.tar.gz | tar --strip-components=1 -xzv
    npm install --only=production
     - install core.json
    
    nano /etc/systemd/system/wings.service
    
    [Unit]
    Description=Pterodactyl Wings Daemon
    After=docker.service
    
    [Service]
    User=root
    #Group=some_group
    WorkingDirectory=/srv/daemon
    LimitNOFILE=4096
    PIDFile=/var/run/wings/daemon.pid
    ExecStart=/usr/bin/node /srv/daemon/src/index.js
    Restart=on-failure
    StartLimitInterval=600
    
    [Install]
    WantedBy=multi-user.target
    
    systemctl enable --now wings
    
    service wings start

###Installation CFG

    git clone https://github.com/Iwhite67/Zilat-CSGO-cfg.git /srv/daemon-data/csgo
    cp -r csgo/ 'id_serv'/csgo

Launch Opt Server
    ./srcds_run +game_mode {{GAME_MODE}} +game_type {{GAME_TYPE}} -game csgo -console -port {{SERVER_PORT}} +ip 0.0.0.0 +map {{SRCDS_MAP}} -strictportbind -norestart +sv_setsteamaccount {{STEAM_ACC}} -tickrate 128 +tv_enable 1 -clientport {{CLIENT_PORT}} +tv_port {{TV_PORT}} -usercon +sv_load_forced_client_names_file "Playername.txt"
