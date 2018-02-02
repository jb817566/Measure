# Measure

Measure is the working name for this prototype. We're not sold on the name yet either. As has been [pointed out](https://martinfowler.com/bliki/TwoHardThings.html), naming things is hard. We're busy thinking of a better name, but if you have a suggestion let us know!

## What is Measure?

At its core Measure is, for lack of a better term, a contributor relationship management system. Measure consists of easy to understand widgets that can be arbitrarily displayed to build dashboards. It allows you to understand how people as individuals and as organizations are interacting with open source projects. It’s metrics that focus not only on code, but on contributors. 

Screenshot: ![MeasureOSS demo](https://github.com/MeasureOSS/Measure/blob/master/assets/img/MeasureOSS-11052017.png)

## Philosophy

* Should be simple
    * We’re willing to trade some flexibility for simplicity
* Should be visually appealing
* Should offer an opinionated default experience, but be extensible
    * We want useful actionable information out of the box, but if you want something different you should be able to easily do so
* Should be able to completely separate inside and outside contributions
* Should treat the concept of contributors as first class citizens
    * Your community is really about the people that create the code


## This is a prototype

Please be aware that this is beta quality software. It should work as expected, but documentation is lacking and setup is still manual. Both issues are important to us and will be addressed as the project matures. That said, we welcome your feedback on both the concept and implementation. Additionally, the metrics we track are still being decided. Feel free to use the issue tracker to suggest the addition, removal, or modification of widgets. We very much want this to be a community effort!

## Initial setup

(most of these steps will get automated. But they aren't, yet.)

Requirements:
* web server with PHP support
* node 6.x.
* git

Check out ghcrawler and -cli:

```
git clone git@github.com:Microsoft/ghcrawler.git
git clone git@github.com:Microsoft/ghcrawler-cli.git
git clone git@github.com:Microsoft/ghcrawler-dashboard.git
```

Get a github access token by going to https://github.com/settings/tokens and creating a token. Give it, for now, all permissions (because I haven't worked out which ones it needs yet). Keep a record of it; you're only shown it once.

Now, start up the crawler:

```
cd ghcrawler
npm install # you only need to do this the first time
cd docker
CRAWLER_GITHUB_TOKENS=<your github token> docker-compose up # this will take a while the first time because it downloads docker images
```

and teach the crawler about your repositories:

```
cd ghcrawler-cli
npm install # you only need to do this the first time
node bin/cc orgs yourorg # e.g., MeasureOSS
node bin/cc tokens "<your github token>#private" # the quotes are important here
node bin/cc queue agithubid/agithubrepo # e.g., MeasureOSS/Measure
node bin/cc start 8
```

Check out the dash maker:

```
Clone this repo
npm install # only need this the first time
cp config.yaml.example config.yaml
# edit config.yaml to contain your list of repositories
node makedash.js
```

and your dashboard should be in `dashboard/index.js` (unless you changed that in config.yaml).
