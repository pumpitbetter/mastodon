# Pump It Better

## Update from upstream

git remote add upstream git@github.com:mastodon/mastodon.git
git fetch upstream
git rebase upstream/main
git push
git push --tags

git checkout pumpitbetter
git merge v3.5.3
git push

## Build and push own docker image

docker build -t pumpitbetter/social:latest .
docker build -t pumpitbetter/social:v3.5.3.8 .
docker push pumpitbetter/social:latest
docker push pumpitbetter/social:v3.5.3.8

## Run local development 
https://www.howtoforge.com/how-to-install-mastodon-social-network-with-docker-on-ubuntu-1804/

cp .env.production.sample .env.production
docker-compose build
SECRET_KEY_BASE=$(docker-compose run --rm web bundle exec rake secret)
sed -i -e "s/SECRET_KEY_BASE=/&${SECRET_KEY_BASE}/" .env.production
OTP_SECRET=$(docker-compose run --rm web bundle exec rake secret)
sed -i -e "s/OTP_SECRET=/&${OTP_SECRET}/" .env.production
PAPERCLIP_SECRET=$(docker-compose run --rm web bundle exec rake secret)
sed -i -e "s/PAPERCLIP_SECRET=/&${PAPERCLIP_SECRET}/" .env.production


docker-compose run --rm web bundle exec rake mastodon:webpush:generate_vapid_key

Open the .env.production file

Search for VAPID_PRIVATE_KEY and VAPID_PUBLIC_KEY in the file and copy the output from the previous command. 




