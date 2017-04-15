# Instance Setup/Research/Walkthrough

This guide assumes you are running Ubuntu 16.04 on an EC2 instance.  It should work elsewhere but this is the only config it has been tested on.
This instance is running 16.04 64-bit with a 30G drive on the free tier.

1. Update/Ugrade

  ``` sudo apt-get update && sudo apt-get upgrade ```

2. Create mastodon user, give them sudo and switch to user
  ```
  sudo adduser --ingroup admin mastodon
  su mastodon
  cd
  ```

3. Dependencies
  ```
  wget -O node_setup_4_x.sh https://deb.nodesource.com/setup_4.x
  sudo chmod +x node_setup_4_x.sh
  sudo bash node_setup_4_x.sh
  sudo apt-get install imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev nodejs file nginx redis-server redis-tools postgresql postgresql-contrib autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev
  sudo npm install -g yarn
  ```

4. Nginx

  Confirm it works
  ``` systemctl status nginx ```

  Check firewall
  ``` sudo ufw app list ```

  Letsencrypt
  ```
  sudo add-apt-repository ppa:certbot/certbot
  sudo apt-get update
  sudo apt-get install certbot
  sudo certbot certonly --rsa-key-size 4096 --register-unsafely-without-email
  ```
  Configure

  see 'etc/nginx/sites-enabled/default', change domain and root settings

  restart nginx
  ```bash
  sudo systemctl restart nginx
  ```
5. Postgres
  ```bash
  sudo -u postgres psql
  CREATE USER mastodon CREATEDB;
  \q
  ```

6. Ruby
  ```bash
  git clone https://github.com/rbenv/rbenv.git ~/.rbenv
  echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(rbenv init -)"' >> ~/.bashrc
  source ~/.bashrc
   ```
 
   confirm the install with
   ``` type rbenv ```
 
   output should be
   ```bash
   rbenv is a function
   rbenv ()
   {
     local command;
     command="$1";
     if [ "$#" -gt 0 ]; then
       shift;
     fi;
     case "$command" in
       rehash | shell)
         eval "$(rbenv "sh-$command" "$@")"
       ;;
       *)
         command rbenv "$command" "$@"
       ;;
     esac
   }
  ```
  
   - setup ruby-build
   ``` git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build ```
 
   - install ruby
   ``` rbenv install 2.4.1 ```
 

7. Clone the repo
```bash
git clone https://github.com/tootsuite/mastodon.git
cd mastodon
```
install dependencies
```bash
gem install bundler
bundle install --deployment --without development test
yarn install
```

  Configure Mastodon

    see .env.production

    run rake 3 times and copy where indicated in .env.production
    ```
    ./bin/rake secret
    ```


8. Setup DB
```bash
RAILS_ENV=production bundle exec rails db:setup
RAILS_ENV=production bundle exec rails assets:precompile
```

## Systemd
see etc/systemd/system/mastodon-*

copy to /etc/systemd/system/

start: ``` sudo systemctl start mastodon-* ```

stop : ``` sudo systemctl stop mastodon-* ```

## Updating

### Normal Update Procedure
in the mastodon folder...
```
git pull
RAILS_ENV=production bundle exec rails db:migrate
RAILS_ENV=production bundle exec rails assets:precompile
sudo systemctl restart mastodon-*
```
### Updating from prior to V1.1.1
in the mastodon folder...
```
rbenv install 2.4.1
git pull
gem install bundler
rbenv rehash
bundle install
yarn install
RAILS_ENV=production bundle exec rails db:migrate
RAILS_ENV=production bundle exec rails assets:precompile
sudo systemctl restart mastodon-*
```

## Admin
https://github.com/tootsuite/mastodon/blob/master/docs/Running-Mastodon/Administration-guide.md


## Resources & References
mastodon: https://github.com/tootsuite/mastodon/blob/master/docs/Running-Mastodon/Production-guide.md

redis: https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04

postgres: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04

ruby: https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-16-04
