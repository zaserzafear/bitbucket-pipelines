# Install Docker
curl https://get.docker.com | sh
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
systemctl status docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version

# Install Harbor
sudo mkdir /harbor
cd /harbor
sudo wget https://github.com/goharbor/harbor/releases/download/v2.6.0/harbor-offline-installer-v2.6.0.tgz
sudo tar xzvf harbor-offline-installer-v2.6.0.tgz
cd harbor

sudo mv ~/harbor/harbor.yml .

sudo ./prepare
sudo ./install.sh

# Add Harbor to systemd
sudo nano /etc/systemd/system/harbor.service

[Unit]
Description=Harbor
After=docker.service systemd-networkd.service systemd-resolved.service
Requires=docker.service
Documentation=http://github.com/vmware/harbor

[Service]
Type=simple
Restart=on-failure
RestartSec=5
ExecStart=/usr/local/bin/docker-compose -f /harbor/harbor/docker-compose.yml up
ExecStop=/usr/local/bin/docker-compose -f /harbor/harbor/docker-compose.yml down

[Install]
WantedBy=multi-user.target

sudo chmod 644 /etc/systemd/system/harbor.service
sudo systemctl daemon-reload
sudo systemctl enable harbor
sudo systemctl start harbor
systemctl status harbor

# Users Roles
Limited Guest: A Limited Guest does not have full read privileges for a project. They can pull images but cannot push, and they cannot see logs or the other members of a project. For example, you can create limited guests for users from different organizations who share access to a project.

Guest: Guest has read-only privilege for a specified project. They can pull and retag images, but cannot push.

Developer: Developer has read and write privileges for a project.

Maintainer: Maintainer has elevated permissions beyond those of ‘Developer’ including the ability to scan images, view replications jobs, and delete images and helm charts.

ProjectAdmin: When creating a new project, you will be assigned the “ProjectAdmin” role to the project. Besides read-write privileges, the “ProjectAdmin” also has some management privileges, such as adding and removing members, starting a vulnerability scan.
