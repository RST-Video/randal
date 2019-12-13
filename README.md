# randal
A simple web service that listens for organization events when a repository has been created and performs actions against the repository. 

## Features
This web service will perform the following actions on newly created repositories when it is notified via an organization event.

* Automate the protection of the master branch
  * Require pull request reviews before merging, 1 approving review 
  * Dismiss stale pull request approvals when new commits are pushed 
  * Require branches to be up to date before merging
  * Force pushes are not allowed
  * Deletions are not allowed
* Notify a user with an @mention in an issue within the repository that outlines the protections that were added

## Installation 
This service has been developed and tested using CentOS 8.0.1905.

These installation steps have been modified from the [Setting up your development environment](https://developer.github.com/apps/quickstart-guides/setting-up-your-development-environment/) and [Using the GitHub API in your app](https://developer.github.com/apps/quickstart-guides/using-the-github-api-in-your-app/) GitHub App Quickstart guides.

### For Development / Demos
For locally hosted development and demos, you may want to leverage the webhook payload delivery service, [smee.io](https://smee.io/).

```
$ sudo yum install npm
$ sudo npm install --global smee-client
```

The usage of smee.io with GitHub apps is outlined in https://developer.github.com/apps/quickstart-guides/setting-up-your-development-environment/#step-1-start-a-new-smee-channel

## Useful Documentation and Links
