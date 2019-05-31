# Working with Panthon's composer flow

If your project has been built with Pantheons [composer/circleci starter build tool](https://pantheon.io/docs/guides/build-tools/) you can user the `config.yml` in this folder as a reference.

## Have you made changes to the default config.yml?

If you generated your project and haven't touched your CircleCi `config.yml` since you can use the one in this folder.

This version of the `config.yml` also maps the Github token created by the build tools generator to the one needed by the analyzer script.

**If you have made NO changes:**
1. Copy the config.yml file in this folder over the one in your project repo.
2. Copy the 2 json files in to the **root** of your project.
3. Adjust the urls in the json files as desired.

Commit and create a PR.  See your new tests in action. :rocket:

**If you have made changes:**

If you have made changes to your `config.yml` you'll need to fit in tests to your flow as appropriate.

Look at the `config.yml` in this folder for [how to get the multidev URL](/examples/pantheon/drupal-composer/config.yml#135) and then use it in later steps. 
