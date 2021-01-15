## Welcome to "Hello World" with GitHub Actions

This course will walk you through writing your first action and using it with a workflow file. 

**Ready to get started? Navigate to the first issue.**

## What are GitHub Actions?
GitHub Actions are a flexible way to automate nearly every aspect of your team's software workflow. Here are just a few of the ways teams are using GitHub Actions:

Automated testing (CI)
Continuous delivery and deployment
Responding to workflow triggers using issues, @ mentions, labels, and more
Triggering code reviews
Managing branches
Triaging issues and pull requests
The sky is truly the limit with GitHub Actions.

The best part, these workflows are stored as code in your repository and easily shared and reused across teams.

To learn even more, check out the GitHub Actions feature page, or the GitHub Actions documentation.

Before you begin
In this course you will work with issues and pull requests, as well as edit files. If these things are not familiar to you, we recommend you take the Introduction to GitHub course, first!

Actions and Workflows
There are two components to using GitHub Actions that we'll cover:

the action itself
a workflow that uses action(s)
A workflow can contain many actions. Each action has its own purpose. We'll put the files relating to the action in their own directories.

Types of Actions
Actions come in two types: container actions and JavaScript actions.

Docker container actions allow the environment to be packaged with the GitHub Actions code and can only execute in the GitHub-Hosted Linux environment.

JavaScript actions decouple the GitHub Actions code from the environment allowing faster execution but accepting greater dependency management responsibility.

**Step 1: Add a Dockerfile**
Our action will use a Docker container so it will require a Dockerfile. Let's add it now. We won't discuss what each line means in detail, but the important thing to know is that the action will be executed in an environment defined by this file.

⌨️ Activity: Create a Dockerfile and open a pull request
Create a file titled action-a/Dockerfile by using this quick link or manually:

Create a new branch. Branches should be named intentionally, so a good name for this branch could be first-action.
On the new branch, create a directory: action-a. Note: If you're working on GitHub.com, you can create a directory and a file at the same time by naming the file action-a/Dockerfile.
In the action-a directory, create a file titled Dockerfile.
Fill the Dockerfile with the content below:

FROM debian:9.5-slim

ADD entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
Commit your file

If you're working locally, you will also need stage your file and to push the branch to GitHub.
Open a pull request with your new branch against main



Nice work, you committed a Dockerfile. You'll notice at the end of the Dockerfile, we refer to an entrypoint script.

ENTRYPOINT ["/entrypoint.sh"]
The entrypoint.sh script will be run in Docker, and it will define what the action is really going to be doing.

**Step 2: Add an entrypoint script**
An entrypoint script must exist in our repository so that Docker has something to execute.

⌨️ Activity: Add an entrypoint script and commit it to your branch
As a part of this branch and pull request, create a file in the /action-a/ directory titled entrypoint.sh. You can do so with this quicklink

Add the following content to the entrypoint.sh file:

#!/bin/sh -l

sh -c "echo Hello world my name is $INPUT_MY_NAME"
Stage and commit the changes

Push the changes to GitHub

Nice work adding the entrypoint.sh script.

In entrypoint.sh, all we're doing is outputting a "Hello world" message using an environment variable called MY_NAME.

Next, we'll define the action.yml file which contains the metadata for our action.

action.yml
All actions require a metadata file that uses YAML syntax. The data in the metadata file defines the inputs, outputs and main entrypoint for your action.

**Step 3: Add an action metadata file**
We will use an input parameter to read in the value of MY_NAME.

⌨️ Activity: Create action.yml
As a part of this branch and pull request, create a file titled action-a/action.yml. You can do so using this quicklink or manually.

Add the following content to the action.yml file:

name: "Hello Actions"
description: "Greet someone"
author: "octocat@github.com"

inputs:
  MY_NAME:
    description: "Who to greet"
    required: true
    default: "World"

runs:
  using: "docker"
  image: "Dockerfile"

branding:
  icon: "mic"
  color: "purple"
Stage and commit the changes

Push the changes to GitHub

Next, we'll define a workflow that uses the GitHub Action.

Workflow Files
Workflows are defined in special files in the .github/workflows directory, named main.yml.

Workflows can execute based on your chosen event. For this lab, we'll be using the push event.

We'll break down each line of the workflow in the next step.

**Step 4: Start your workflow file**
First, we'll add the structure of the workflow.

⌨️ Activity: Name and trigger your workflow
Create a file titled .github/workflows/main.yml. You can do so using this quicklink or manually:
As a part of this branch and pull request, create a workflows directory nested inside the .github directory.
In the new .github/workflows/ directory, create a file titled main.yml
Add the following content to the main.yml file:
name: A workflow for my Hello World file
on: push
Stage and commit the changes
Push the changes to GitHub

Nice work! 🎉 You added a workflow!

Here's what it means:

name: A workflow for my Hello World file gives your workflow a name. This name appears on any pull request or in the Actions tab. The name is especially useful when there are multiple workflows in your repository.
on: push indicates that your workflow will execute anytime code is pushed to your repository, using the push event.
Next, we need to specify a job or jobs to run.

Actions
Workflows piece together jobs, and jobs piece together steps. We'll now create a job that runs an action. Actions can be used from within the same repository, from any other public repository, or from a published Docker container image. We'll use an action that we'll define in this repository.

We'll add the block now, and break it down in the next step.

**Step 5: Run an action from your workflow file**
Let's add the expected action to the workflow.

⌨️ Activity: Add an action block to your workflow file
As a part of this branch and pull request, edit .github/workflows/main.yml to append the following content:
jobs:
  build:
    name: Hello world action
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: ./action-a
        with:
          MY_NAME: "Mona"
Click Start commit in the top right of the workflow editor
Type your commit message and commit your changes directly to your branch

Nice, you just added an action block to your workflow file! Here are some important details about why each part of the block exists and what each part does.

jobs: is the base component of a workflow run
build: is the identifier we're attaching to this job
name: is the name of the job, this is displayed on GitHub when the workflow is running
runs-on: defines the type of machine to run the job on. The machine can be either a GitHub-hosted runner or a self-hosted runner.
steps: the linear sequence of operations that make up a job
uses: actions/checkout@v1 uses a community action called checkout to allow the workflow to access the contents of the repository
uses: ./action-a provides the relative path to the action we created in the action-a directory of the repository
with: is used to specify the input variables that will be available to your action in the runtime environment. In this case, the input variable is MY_NAME, and it is currently initialized to "Mona".
Your action has been triggered!
Your repository now contains an action (defined in the /action-a/ folder) and a workflow (defined in the ./github/workflows/main.yml file).

This action will run any time a new commit is created or pushed to the remote repository. Since you just created a commit, the workflow should have been triggered. This might take a few minutes since it's the first time running in this repository.

Seeing your Action in action
The status of your action is shown here in the pull request (look for All checks have passed below), or you can click the "Actions" tab in your repository. From there you will see the actions that have run, and you can click on the action's "Log" link to view details.

View an action's log

**Step 6: Trigger the workflow**
⌨️ Activity: See your action trigger the workflow
You've done the work, now sit back and see your action trigger the workflow!

Success! 🎉 Your workflow ran! You can see the output here.

You should see the string "Hello world, I'm Mona!" printed at the bottom to stdout.

Actions successful log file

**Step 7: Incorporate the workflow**
As a final step, merge this pull request so the action will be a part of the main branch.

Anyone that uses this repository, and any future code can benefit from this workflow and your new action!

⌨️ Activity: Merge your workflow into the main branch
Merge this pull request
Delete your branch
