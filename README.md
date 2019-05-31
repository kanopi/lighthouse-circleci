# CircleCi and Lighthouse Integration 

Here is an example of how  you can integrate [CircleCI](https://circleci.com/) and [Google Lighthouse](https://github.com/GoogleChrome/lighthouse) as part of your development flow to get performance regression checks on each pull request.

A lot of this is built off of the work [Stuart Sandine](https://github.com/stuartsan) and the [blog post](https://stuartsandine.com/lighthouse-circle-ci/) he wrote. One of the things that I wanted to take further is make sure the process is more modular and can be dropped in to an existing flow without too much effort.  For example his analyze scores script is built in to the project specific repo but I've moved it out in to a custom docker image.       

Another thanks goes out to [Sean Dietrich](https://github.com/sean-e-dietrich) for getting me going on the Docker stuff and the tooling he's built in to the Kanopi [CI Project](https://github.com/kanopi/ci-tools).

## What you'll need

* [Github token](https://github.com/settings/tokens)
   * This is to be added to CircleCI as an environment variable for the project.
   * Variable should be named `GH_AUTH_TOKEN` in CircleCI.  
   * This lets [CircleCI Bot](https://www.npmjs.com/package/circle-github-bot) write the comments on the PRs
* [Enable Github checks](https://circleci.com/docs/2.0/enable-checks/) integration for the repo
* [lighthouse.json](lighthouse.json) file
   * Should be located in the root of the project.
   * This is a json file used in CircleCI to know which URL to test and the minimum score to hit.
   * Technically this could be named anything but if you name your file this the config example should "just work".

## How it works

If you look at the [config.yml](.circleci/config.yml) in this repo you'll see it's essentially broken down in to two jobs.  The first runs the Lighthouse tests against a URL and then second analyses the results.  The lighthouse job is separate so we can run it multiple times and average the difference.

The second major part of this is the Docker image (`kanopi/ci:edge-lighthouse`).  This is the custom image we've built that has Lighthouse and the analyzer script baked in to it so this process is lightweight on the individual project side.

### Basic setup

So in the most basic example you can simply:

1. Setup the token and github checks on your repo.
2. Copy the example part of config.yml for `simpleTests` and `processResultsSimpleTests` jobs
3. Copy the example part of config.yml for `simpleLighthouseBenchmark` workflows
3. Copy lighthouse.json file and adjust the URL to what you want to test.
 
This will get you testing a specific URL on every PR.

### Relative URLs

In more complicated development flows you'll want to test against specific environments so you can't hard code an absolute URL in the json file.

There are 3 things we'll need to update compared to the simple example.

1. Updating the lighthouse.json file to flag it's use of relative urls; so `is_relative_url` is now `true`;
2. Updating the config to pass a base url to the lighthouse testing script
3. Setting an environment URL for the analyzing script so the links in github comments work correctly 


The analyzer script is configured to look for an environment variable called `LIGHTHOUSE_BASE_URL` and when that exists will prepend that to the testing URL for notifications in github.





**Add authenticated tests**

In the original example from Stuart Sandine he'd gotten some authenticated tests working. Because the analyzer script with written to deal specifically with that use case for his demo app it didn't really make sense to include at this time.  

I'd like to add a way to authenticate against Wordpress and Drupal sites simply as those are the primary CMS's we develop in.

**Smaller Docker Image**

There is potential to build a custom Docker image just for the analyzer script and Lighthouse so it doesn't take so long to download in CircleCI 
