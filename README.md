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

### Pre-requisites
1. Register and configure how you are going to access your GitHub App using randall. (https://developer.github.com/apps/quickstart-guides/setting-up-your-development-environment/#step-2-register-a-new-github-app). The App should have the following permissions.
   * Repository permissions - Administration: Read & write
   * Repository permissions - Contents: Read & write
   * Subscribe to events - Repository
   * The GitHub app should be installed "Only on this account"
2. Save your private key and App ID (https://developer.github.com/apps/quickstart-guides/setting-up-your-development-environment/#step-3-save-your-private-key-and-app-id)
3. Install App to your GitHub organization
   1. In the Settings of your GitHub App, click "Install App"
   2. Click "Install" button for your organization account
   3. Make sure "All repositories" is selected and the permissions are
      * Write access to code
      * Read access to metadata
      * Read and write access to administration
   4. Click "Install"
4. Obtain the randal web service and place it in the directory you want to run it from.
   ```
   git clone https://github.com/RST-Video/randal.git
   ```
5. Install CentOS 8's package for the ruby gem, bundle, and its dependencies.
   ```
   $ sudo yum install rubygem-bundler
   ```
   
### Service configuration
1. Prepare the runtime environment using the environment setup steps in https://developer.github.com/apps/quickstart-guides/setting-up-your-development-environment/#step-4-prepare-the-runtime-environment. Make sure you have the following from the Pre-requisite steps
   * GitHub App private key
   * GitHub App ID
   * GitHub App's Webhook secret 
2. Edit the randal_service.rb file and change the '@' user on Line 115 to a GitHub user that should be receiving notifications when automated branch protection for the repository has been applied. This file will also allow you to change the IP address and port the service runs on, if desired.
3. Get the ruby gems needed for running the randal service. As the user that will be running the service, change to the directory that the randle service files exist and run
   ```
   bundle install
   ```

## Running the service
From the directory that has the randal_service.rb file.
```
ruby randal_service.rb
```
When creating new repositories in GitHub, you should see the "master" branch now has protections, and an Issue should be generated notifying the specified GitHub user along with a summary of protections applied to the "master" branch.

## Additional useful documentation and links
* [Sinatra documentation](https://github.com/sinatra/sinatra#table-of-contents)
* [GitHub Webhooks](https://developer.github.com/webhooks/)
* [GitHub API Reference](https://developer.github.com/v3/)
* [Branch Protection with Oktokit](https://www.rubydoc.info/gems/octokit/Octokit/Client/Repositories#protect_branch-instance_method)
* [Creating Repository Issues with Oktokit](https://www.rubydoc.info/gems/octokit/Octokit/Client/Issues#create_issue-instance_method)
* Using custom media types and headers
  * https://rubydoc.info/gems/sinatra/Sinatra/Helpers#headers-instance_method
  * https://stackoverflow.com/questions/25660827/accessing-headers-from-sinatra/35689150
* [GitHub writing and formatting syntax](https://help.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax)
* [Ruby Style Guide](https://shopify.github.io/ruby-style-guide/)
