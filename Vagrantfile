$script = <<-SCRIPT
apt-get update
apt-get install git docker docker-compose -y
systemctl enable docker && systemctl start docker
chown -R vagrant:vagrant /media
git clone https://github.com/michielcox/good-bot.git
chown -R vagrant:vagrant good-bot
cd good-bot
docker-compose up -d
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision "shell", inline: $script
  config.vm.network "public_network", ip: "192.168.1.66", bridge: "en0: Ethernet"
  config.vm.provision "shell", # default router
    run: "always",
    inline: "route add default gw 192.168.1.1"
  config.vm.provision "shell", # delete default gw on eth0
    run: "always",
    inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"

  config.vm.synced_folder "/Volumes/vagrant", "/media", :mount_options => ["dmode=777", "fmode=666"]
  config.vm.network "forwarded_port", guest: 8112, host: 8112   #Deluge
  config.vm.network "forwarded_port", guest: 9117, host: 9117   #Jackett
  config.vm.network "forwarded_port", guest: 6789, host: 6789   #nzbget
  config.vm.network "forwarded_port", guest: 32400, host: 32400 #Plex
  config.vm.network "forwarded_port", guest: 8989, host: 8989   #Sonarr
  config.vm.network "forwarded_port", guest: 7878, host: 7878   #Radarr
  config.vm.network "forwarded_port", guest: 6767, host: 6767   #Bazarr


end
