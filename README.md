# wwnz-github-actions-example
Example of setting up github actions and running them locally using nektos/act.

First, have a read of [act](https://github.com/nektos/act), and follow the relevant steps to install it on your machine. act has a prerequisite of **Docker** being installed on your machine (and being currently running) so that it is able to run the containers that will execute your actions locally.

This guide does not include how to install Docker, WSL, etc. But there may be a hint [here](https://woolworths-agile.atlassian.net/wiki/spaces/WDEC/pages/32133284510/Windows+10+-+using+WSL).

### Running local actions

It is best to read through the act documentation (most of what you need to know can be found in the [act repo](https://github.com/nektos/act) README)

But if you just want a quick-start (after installing of course), simply run `act push` after checking out this repo.

Replace `push` with whatever action the workflow should trigger on. Also note what branch you are running on if the branch is important to triggering the workflow!

### Running with secrets file

```
act [action] --secret-file githubsecrets.env
eg: `act push --secret-file githubsecrets.env`
```

## Other information for people unfamiliar with Github Actions in general

There is plenty of documentation on Github Actions, but just a tip for how to think about github actions:

* There is nothing particularly special about how github actions run. You define steps, which run in the type of environment you specify.
* The type of environment you define will probably be something like `"ubuntu-latest"`. Just think of your github action code as running on any random Ubuntu machine.
* If you can run it in Ubuntu, you can probably run it in github actions (eg: bash, python, C, etc). You just need to make sure it's installed.
* The machine you specify doesn't have any special permissions that you don't have. It can only get permission to do things by providing it with service accounts (though secrets) and permissions on those accounts.


### Running actions that require service accounts and such

The docker container in which the actions are running don't know much at all about your local environment, including your gcloud login information. If you want to deploy things to a test project, or interact with GCP services, you will need to provide act with the same secrets as would be be normally found in Github Actions themselves.

An example of this, is we commonly use a secret in github repos called `SERVICE_ACCOUNT_KEY`, which is a JSON blob of a service account which can execute the necessary actions required by the workflows. If you want to be able to test locally, you will need to get a similar service account key, or create a key for your own account which has the necessary permissions to do so.

Save that blob into your `githubsecrets.env` _**(BUT NEVER COMMIT SECRETS TO GITHUB!)**_ or other aptly named file. The variable name for the secret should match the one you will be using in github (ie: `SERVICE_ACCOUNT_KEY`, or whatever you plan on naming it). Now the action should run just like (or similarly to) how it would run on github.