# RPi Docker Starter
Basic Docker media server template with setup instructions.

# Steps
## RPi Setup
1. Install OS on a microSD using [Raspberry Pi Imager](https://www.raspberrypi.com/software/)  
  *You can configure net SSH access and network before installation*
1. SSH into the RPi: `ssh 192.168.x.x@user`  
  *You can run `sudo raspi-config` to finish setting up your device*

## Docker Setup
1. Update RPi: `sudo apt update && sudo apt upgrade`
1. Run Docker setup: `curl -sSL https://get.docker.com | sh`  
  *Typically not recommended, but Docker is a trusted source*
1. Add user to the Docker group: `sudo usermod -aG docker [user]`  
  Or `sudo usermod -aG docker ${USER}` to add current user
  *You can check it by running `groups ${USER}`*
1. Restart to apply changes: `sudo restart`
1. SSH to the RPi again
1. Run a test container: `docker run hello-world`  
  *It should tell you whether your setup is running correctly*

## Compose Setup
This is an optional step, but recommended.  
1. Install `pip3`:   
  *`python3` & `pip3` are required to setup and run `docker-compose`*  
  ```bash
  sudo apt-get install libffi-dev libssl-dev
  sudo apt install python3-dev
  sudo apt-get install -y python3 python3-pip
  ```
1. Update `pip`: `pip install --upgrade pip`
  *Usually not necessary*
3. Install `docker-compose`: `sudo pip3 install docker-compose`  
  *Check status using `docker version` & `docker-compose version`  
  *[^1]NOTE: if you get `Failed to build bcrypt cryptography`,  
  try `sudo apt install docker-compose` instead.*

[^1]: See [github.com/adriankumpf](https://github.com/adriankumpf/teslamate/discussions/2881)  

## Environment Setup
1. Enable Docker system service: `sudo systemctl enable docker`  
  *This allows containers to start on boot*  
1. Prepare docker directory for compose files and permanent container storage  
  *Following is my recommendation*  
  ```
  .
  ├── srv                       
  │   ├── docker                      # Docker root
  │   │   ├── appdata                 # Container local directory mounts
  │   │   │   ├── container_name      # Dirs must be created before starting given container
  │   │   │   ├── container_name-db
  │   │   │   └── ...
  │   │   ├── env                     # If you want to use multiple .env files
  │   │   ├── secrets                 # Docker secrets
  │   │   ├── shared                  # Shared certificates
  │   │   └── docker-compose.yml      # Compose config (if using compose)
  |   └── ...
  └── ...
  ```
  ```bash
  cd /srv
  sudo mkdir docker
  chown ${USER} docker  
  cd docker
  mkdir appdata env secrets shared
  ```

## Launch

# Troubleshooting
## `Cannot connect to the Docker daemon at unix:/var/run/docker.sock. Is the docker daemon running?`
Try `sudo` your command. If that doesn't help, try `sudo service docker restart`.

# Links
## Sources
[pimylifeup.com](https://pimylifeup.com/raspberry-pi-docker/)  
[dev.to/elalemanyo](https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo)  

## Useful
[Static IP - pimylifeup.com](https://pimylifeup.com/raspberry-pi-static-ip-address/)  
