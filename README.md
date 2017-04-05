# mastodon-docs

## Instance Setup/Research/Walkthrough

1. Setup Ubuntu 16.04 on AWS

  ```bash
  sudo apt-get update && sudo apt-get upgrade 
  ```
  
2. Clone the repo

  ```bash
  git clone https://github.com/tootsuite/mastodon.git 
  ```

3. Follow https://github.com/tootsuite/mastodon/blob/master/docs/Running-Mastodon/Production-guide.md

  ```bash
  curl -sL https://deb.nodesource.com/setup_4.x | sudo bash - 
  ```

  ```bash
  sudo apt-get install imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev nodejs file
  ```

  ```bash
  sudo npm install -g yarn 
  ```
4. nginx: https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04

  ```bash
  sudo apt-get install nginx
  ```
  
5. letsencrypt: https://certbot.eff.org/#ubuntuxenial-nginx

  ```bash
  sudo add-apt-repository ppa:certbot/certbot
  ```
  
  ```bash
  sudo apt-get update
  ```
  
  ```bash
  sudo apt-get install certbot
  ```
  
  ```bash
  sudo certbot certonly
  ```
  
6. redis: https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04

  ```bash
  sudo apt-get install redis-server redis-tools
  ```
  
