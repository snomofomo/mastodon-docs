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
