# Installing Docker to the Raspberry Pi

Thanks to a nifty install script developed by the Docker team, installing the container software is incredibly simple.

You can complete the following steps by using an SSH connection to your Raspberry Pi.

1. Our first task is to update all our existing packages and then upgrade all existing packages before we proceed to install Docker.

We can do this by running the following two commands on the Raspberry Pi.

`sudo apt update`

`sudo apt upgrade`

2. With our Raspberry Pi entirely up to date, we can now go ahead and install Docker to the Raspberry Pi.

You can download and run the official Docker setup script by running the following command.

`curl -sSL https://get.docker.com | sh`

This script can take some time to complete as it automatically detects and installs everything it needs to run Docker on the Raspberry Pi.

# Setting up your User for Docker

We need to adjust our user group before we can start using Docker.

1. We can use the usermod command and the “$USER” variable to insert the current user into the docker group.

`sudo usermod -aG docker $USER`

2. Since we made some changes to our user, we will now need to log out and log back in for it to take effect.

You can log out by running the following command in the terminal.

`logout`

3. Once you have logged back in, you can verify that the docker group has been successfully added to your user by running the following command.

`groups`

This command will list out all the groups that the current user is a part of. If everything worked as it should, the group docker should be listed here.

# Installing Spoolman

1. Make a directory for spoolman

`mkdir spoolman`

2. Change to that directory

`cd spoolman`

3. Make a data directory and set the owner

`mkdir data`

`chown 1000:1000 data`

4. Make docker-compose.yml file

`nano docker-compose.yml`

5. Paste the lines below, hit CTRL+X and hit Y and Enter to save

```
version: '3.8'
services:
  spoolman:
    image: ghcr.io/donkie/spoolman:latest
    restart: unless-stopped
    volumes:
      # Mount the host machine's ./data directory into the container's /home/app/.local/share/spoolman directory
      - type: bind
        source: ./data
        target: /home/app/.local/share/spoolman
    ports:
      # Map the host machine's port 7912 to the container's port 8000
      - "7912:8000"
    environment:
      - TZ=America/New_York
```

6. Start the docker container

`docker compose up -d`

7. Once the container has started, open a web browser with the address of the pi followed by `:7912`. For example, if your klipper webpage is `http://klipper.local` then spoolman is at `http://klipper.local:7912`
