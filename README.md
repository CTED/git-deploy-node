git-deploy-node
===============

Deploy a node app using git for continuous deployment.

## Introduction

We will setup a remote repository just for deployments on the production server.

When you are finished, you will be able to do continuous deployment using the following command (from your local machine):

    git push production master

### Setup production server

	ssh ubuntu@ec2-52-86-63-176.compute-1.amazonaws.com

This is where you will deploy to

	mkdir -p ~/gitdeploy/apps/livetracker

This is where the post-receive hook will export production code to (and served from by nginx)

	mkdir -p /var/www/nodejs/livetracker
	

Setup deploy branch

	cd ~/gitdeploy/apps/livetracker
	git init --bare

Copy post-receive hook into repo

	cp ~/projects/git-deploy-node/post-receive ~/deploy/example.com/hooks/

Modify post-receive as needed. This is your build process.

	npm install
	NODE_ENV=production grunt deploy

### Setup local dev machine (laptop)

*Assumption: You've already set up your ssh key and you can ssh into the server without specifying the key*

Add a remote named 'production' to deploy to from local working copy (this is where you will deploy from).

	git remote add production ubuntu@ec2-52-86-63-176.compute-1.amazonaws.com:~/gitdeploy/apps/livetracker

List remotes

	git remote -v

Deploy master branch to production

	git push production master


