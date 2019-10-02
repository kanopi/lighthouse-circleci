# Working with an existing Pantheon Drupal 7 site

You may have en existing Drupal 7 site on Pantheon and those don't come with the CircleCi setup out of the box like the Drupal 8 Composer projects.

Following these directions you can get the MultiDev deployment flow and Lighthouse audits 

## Setup

There are more manual steps for this as it's not taken care of compared to the D8 build tools process.

1. There is a `.circleci` folder within this one that can be copied and pasted in to yours. This includes the Lighthouse JSON files and Pantheon scripts.
2. In CircleCI setup for your repo you'll need to do a few things
    1. Add environment variables
        * `GITHUB_TOKEN` https://github.com/settings/tokens
        * `GIT_EMAIL` This is the email of someone that has access to the repo in Github and is used for the commit messages.
        * `TERMINUS_SITE` The site name in Pantheon
        * `TERMINUS_TOKEN` A machine token from Pantheon that has access to the site
    2. Add a private key   
        * This key needs to be able to access the Github repo so the public key should be added to the Github user used in the `GIT_EMAIL` variable.
    
Once this is done you should be able see the MultiDev creation and Lighthouse audits in the pull requests.

        

 
