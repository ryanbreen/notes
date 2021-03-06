# Umm... I have to do WHAT to run this code?
* Gord Tanner (Co-Founder / CTO) of bitHound [@gordtanner]

* Was able to get new developers at bitHound able to open PRs within their first day

# Same problems with open vs. close source

## Nothing you don't know already
* This isn't going to be teaching us anything new, we already know most of this stuff

## Where is your javascript?
* Not much javascript in this talk

# Welcome to the team
* Think about the first time you've pulled down code to work on (work, open source, etc.)
* Setup your laptop
  * Go to wiki to figure out how to set it up
  * Wiki hasn't been updated for over a year
  * Had to find how to download SQL, no one had the CD anymore, it was a pain
  * A step-by-step process that took an entire week
* When you have new employees update this wiki, you have people with no experience and no knowledge of code writing documentation (bad idea)

## First impressions matter
* People's experience on their first day at your org/project matters
  * It sets the tone for their perceived quality of your code

## Set up for success
* Want to build on first-time enthusiasm

# Documentation
* Not an exciting topic, but extremely extremely important

## README.md
* Github/Bitbucket creation
* Brought documentation to the fore-front of a project
* Any documentaton not in README should at least be linked from README

## Whiteboard sessions
* We have ideas of mental models of our infrastructure
  * This is really important time to spend with people
  * Not easy to expose for closed source
  * DOn't really want it in a permanent place because it will quickly get out of date

## Command line tooling
* Good way of showing the things you can do

## Code Comments
* There are a few ways to comment your code
  * Comment what it does
  * Comment why it does that (better)
* We have ways to figure out what the code does -> it's called code
* Comments should cover why, the stuff you can't figure out from looking at the code
* Document why the fugly parts of your code are fugly

# Vagrant (kind of a love story)
* Allows you to have a standard virtualized environment to do your coding in
* Bithound has highly distributed system

## Multiple Servers
* Virtualize a mini-version of what production is
* What you have doing locally doesn't work the same on hundreds of servers
* Local developers are now using what's actually in production

## Hide the fact you are using Vagrant
* Only tell people that you're using it when you have to
* Use things like port-forwarding to achieve this

```
$*
vagrant ssh --command ""
```

$* (splat) allows all the arguments you pass into one command to be passed into the next command

## No one expects perfection
* Running `./configure` sets up everything at bitHound

## Setting up mongo to allow remote connections:
```
apt-get install -y mongodb
perl 's/^bind_ip/#bind_ip/g' /etc/mongodb.conf
sudo service mongodb restart
```

* People may think vagrant is slow

## I'm not sure what this does, but it's supposed to help fix a confusing problem by running in all Vagrant environments
```
git config --global http.postBuffer 524288000
```
* 6 months later someone will find this (with a comment explaining why it's there) and will be better prepared to understand why

## Fix the clock
* Clock drifting can make OAuth failed, and make people think that it was their fault somehow:
```
/etc/init.d/ntp stop
set +e
ntupdate pool.ntp.org
set -e
/etc/init.d/ntp start
```

## Up our per-use file limit
```
some helper script thing
```

## Nag your users
* Keep everyone working in the same environment

## Live Reload
* I can't enough about this
* nodemon, livereload, grunt-livereload, etc.
* Allows you to edit your code and
  * Restart browser
  * Restart services
* Avoids having to know all the things to restart

## Tests
* More than just unit tests
* How do you "test" the system
* We forget to instill into new people how to test the system
  * Share all those tricks and workarounds you've used to test the system
* Someone complains "It took me 6 hours to wait for this thing"
  * Reponse "oh, just use --dontDoThat argument"

# Always Keep Learning
* Keep thinking about what it takes to go from a brand new machine to work on your project
