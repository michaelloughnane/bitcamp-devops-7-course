

# Welcome to the course!

We'll learn how to create a workflow that enables Continuous Delivery with the help of Microsoft Azure. You'll:
- create a workflow to deploy to staging based on a label
- create a workflow to deploy to production based on merging to master

### A Reminder: What is Continuous Delivery?

[Martin Fowler](https://martinfowler.com/bliki/ContinuousDelivery.html) defined Continuous Delivery very simply in a 2013 post as follows:

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.

A lot of things go into delivering "continuously". These things can range from culture and behavior to specific automation. In this course, we're going to focus on deployment automation.

### Setting up environments and kicking off deployments

Automation works at its best when a set of triggers work harmoniously to set up and deploy to target environments. Engineers at many companies, like at GitHub, typically use a ChatOps command as a trigger. The trigger itself isn't incredibly important.

In our use case, we'll use labels as triggers for multiple tasks:
- When someone applies a "spin up environment" label to a pull request, that'll tell GitHub Actions that we'd like to set up our resources on an Azure environment.
- When someone applies a "stage" label to a pull request, that'll be our indicator that we'd like to deploy our application to a staging environment.
- When someone applies a "destroy environment" label to a pull request, we'll tear down any resources that are running on our Azure account.

For now, we'll focus on staging. We'll spin up and destroy our environment in a later step.

## Step 1: Configure a trigger based on labels

In a GitHub Actions workflow, the `on` step defines what causes the workflow to run. In this case, we want the workflow to run whenever a label is applied to the pull request.

### :keyboard: Activity: Configure the workflow trigger based on a label being added

1. Edit the `deploy-staging.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/staging-workflow/.github/CHANGETHIS/deploy-staging.yml?) _(We recommend opening the quick link in another tab)_

2. Change the name of the directory `CHANGETHIS` to `workflows`, so the title of this file with the path is `.github/workflows/deploy-staging.yml`. If you're working on the GitHub.com user interface, see the suggestion below for how to rename.

3. Edit the contents of this file to trigger a job called `build` on a label

4.  Add a block for environment variables before your jobs, as follows:

   ```YML
   env:
     DOCKER_IMAGE_NAME: USER-azure-ttt
     IMAGE_REGISTRY_URL: docker.pkg.github.com
     #################################################
     ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
     #################################################
     #################################################
     ### REPLACE USERNAME WITH GH USERNAME         ###
     AZURE_WEBAPP_NAME: USER-ttt-app
     #################################################
   ```

<details><summary>Changing the name of a directory</summary>

If you're working locally, rename your file or drag-and-drop the file into the proper directory as you'd normally do. If you're working on GitHub.com, you can change the name of a directory by changing its filename and using the Backspace key until you reach the directory, as follows:
1. In the file name field, click in front of the first character in the file name
2. Press <kbd>Backspace</kbd> or <kbd>Delete</kbd> on your keyboard until you see the path you want to keep. You can continue to press <kbd>Backspace</kbd> even if the text box with the file name is empty. 
2. Enter the new name for your directory.
3. Enter `/` to let GitHub know this should be a new directory.
</details>

Your resulting file should be in `.github/workflows/deploy-staging.yml` and it should look like this:

```yml
name: Stage the app

on: 
  pull_request:
    types: [labeled]

env:
  DOCKER_IMAGE_NAME: USER-azure-ttt
  IMAGE_REGISTRY_URL: docker.pkg.github.com
  #################################################
  ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
  #################################################
  #################################################
  ### REPLACE USERNAME WITH GH USERNAME         ###
  AZURE_WEBAPP_NAME: USER-ttt-app
  #################################################
jobs:
  build:
    runs-on: ubuntu-latest
```

---

I'll respond when you push a commit on this branch.
