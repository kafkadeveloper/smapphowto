# How to setup streaming collections

##(1) - Identify the machine the collection should run on.

This can be a digital ocean server, a local server (like a workstation or research box), or whatever. Just make sure that 1. There's room for another collection on the machine (machines can handle about a max of 8-9) per IP address. 

##(2) - Go onto twitter app console, create app, get auth info.

https://apps.twitter.com/

then press `Create New App`

##(3) - Log into server with ssh and connect to NYU's vpn (if outside nyu).

If you're new there's a great [digital ocean guide on ssh](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-in-ubuntu).

I also wrote a separate guide on how to command line connect to [NYU's vpn].(https://github.com/SMAPPNYU/smapphowto/blob/master/howto_connect_to_nyuvpn.md)

##(4) - Setup a keyed login to hades using ssh-copy-id.

Check Operating System by typing `uname -a`.

If you're running linux: you have ssh-copy-id.

`ssh-copy-id username@remote_host`

enter your password etc.

##(5) - Create the filter criteria file.

Use this [script](https://github.com/SMAPPNYU/shellscripts#makefiltercriteriash)

or 

just use vi or nano to make a file with path:

~/COLLECTIONAME_fc.json

##(6) - Create the oauth file. 

Use this [script](https://github.com/SMAPPNYU/shellscripts#makeoauthsh)

ir

just use vi or nano to make a file with path:

~/COLLECTIONAME.oauth.json 

##(7) - Create the 'collector' virtualenvironment if it isn't there yet.

Check for virtualenv by typing `pip show virtualenv` should return something like:

```bash
USER@HOSTNAME:~$ pip show virtualenv
---
Name: virtualenv
Version: 13.1.2
Location: /usr/local/lib/python2.7/dist-packages
Requires: 
USER@HOSTNAME:~$ 
```

If nothing is returned then setup virtualenv:

Setup virtual env, `pip install virtualenv` if it's not installed. 

Create teh directory at path ~/.venvs if it doesn't exist:

`mkdir ~/.venvs`

then go inside `cd .venvs`

then run `virtualenv collector`

once it's setup run `source ~/.venvs/collector/bin/activate`

you should see `(collector)` appear in parentheses at the base of your terminal prompt before your name like `(collector)user@host`.

then navigate like so `cd ~/TweetCollector`

then run `pip install -r requirements.txt`

then leave the virtual environment by typing `deactivate`

##(8) - Run `~/TweetCollector/start_new_collection.py`

Should look something like this:

```bash
~/.venvs/collector/bin/python ~/TweetCollector/start_new_collection.py -s localhost -p 27017 -u ADMINUSER -w PASSWORD -f ~/filter_criteria/COLLECTIONNAME_fc.json -o ~/oauth/COLLECTIONNAME_fc.oauth.json -d COLLECTIONNAME_fc --shard
```

you should see an 'appending output to nohup.out' appear in console

and in the crontab you should see an entry for that tweet collector.

and in the process tab (ps aux | grep COLLECTIONNAME)

if you check philosoraptor dashboard you should see your collection:

http://philosoraptor.bio.nyu.edu/dashboard

This does not mean it was successfully created though.

On the dashboard, check to make sure the filter criteria are set for your collection and that it is capable of collecting tweets (tweet something that qualifies the filter and see if it appears on dashboard).

If there's nothing there. Then check to make sure the ssh tunnel to hades is good.

Note: see virtualenv guide here - http://docs.python-guide.org/en/latest/dev/virtualenvs/

Note: Steps one 4-6 were supposed to be solved by this script but it stopped working on some systems:

https://github.com/SMAPPNYU/shellscripts#setupshardtweetcollectorsh

###authors

written by <a href="https://github.com/yvan">Yvan Scher</a>



