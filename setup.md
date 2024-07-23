- Setup NPM/Nodejs in container
```shell
docker run --name ubuntu-nodejs -i -d ubuntu:latest
docker exec -it ubuntu-nodejs bash

apt update && export DEBIAN_FRONTEND=noninteractive && apt install -y git curl nano apt-utils gcc g++ make

export NPMVER=20
curl -fsSL https://deb.nodesource.com/setup_$NPMVER.x | bash -
apt-get install -y nodejs
npm -version


```
- Clone and modify repository
```shell
export repo=your-repo-url
git clone https://github.com/bkimminich/juice-shop.git && cd juice-shop
git checkout -b main tags/v17.0.0
git rm -rf .github 
git rm -rf .dependabot
git rm .npmrc
nano ./frontend/.npmrc
## replace package-lock=false with legacy-peer-deps=true
git add ./frontend/.npmrc
nano .gitignore
## remove package-lock.json
git add .gitignore
nano ./frontend/.gitignore
## remove package-lock.json
git add ./frontend/.gitignore
npm install --ignore-scripts
git add package-lock.json
cd frontend && npm install --ignore-scripts
git add package-lock.json
cd .. && nano setup.md 
### copy this text and add
git add setup.md
git commit -a -m "lockfiles & legacy-peer-deps"
git remote set-url origin $repo
git push --set-upstream origin main
```