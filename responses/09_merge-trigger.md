# Different triggers

Deployments to production can be manual (like through a Chat Ops command), or automated (if, say, we trust our pull request review process and we've followed continuous integration practices).

We'll trigger a deployment to the production environment whenever something is committed to master. Our master branch is protected, so the only way for commits to appear on master is for a pull request to have been created and gone through the proper review process and merged.

## Step 8: Write the production deployment trigger

Let's create a new workflow that deals specifically with commits to master and handles deployments to prod.

### :keyboard: Activity: Write the production deployment trigger on merge to master

1. Edit the `deploy-prod.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/production-deployment-workflow/.github/CHANGETHIS/deploy-prod.yml?) _(We recommend opening the quick link in another tab)_
2. Rename the file in this pull request to `.github/workflows/deploy-prod.yml`
3. Add a `push` trigger
4. Add `branches` inside the push block
5. Add `- master` inside the branches block
6. Add the same environment block as before:
    ```yaml
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
7. Commit your changes to this branch

The file should look like this when you're finished:

```yml
name: Production deployment

on: 
  push:
    branches:
      - master

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

---

I'll respond when you push a commit on this branch.
