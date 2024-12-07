# Gitlab-ci-cd-process
##  pipeline script must me saved .gitlab-ci.yml in Gitlab
- This GitLab CI/CD pipeline configuration file defines four jobs, organized in three stages: build, test, and deploy. Here's a detailed breakdown:
### 1. Stages
```bash
stages:
  - build
  - test
  - deploy
```
`stages`: This defines the order in which the jobs will run. Jobs in the build stage run first, followed by jobs in the test stage, and finally jobs in the deploy stage.
### 2. Build Job (build-job)
```bash
build-job:
  stage: build
  tags:
    - ci/cd
  script:
    - echo "Hello, $GITLAB_USER_LOGIN!"
```
`stage`: build: The job is assigned to the build stage, so it runs first in the pipeline.<br>
`tags`: The job will only run on a runner that is tagged with ci/cd. This helps assign the job to a specific runner (e.g., one configured for CI/CD tasks).<br>
`script`: This defines the commands that the runner will execute when this job runs. Here, it echoes a message containing the GitLab user login name ($GITLAB_USER_LOGIN), which is a predefined GitLab CI/CD environment variable representing the username of the person who triggered the pipeline.<br>
### 3. Test Job 1 (test-job1)
```bash
test-job1:
  stage: test
  tags:
    - ci/cd
  script:
    - echo "This job tests something"
```
`stage: test`: Th  is job is part of the test stage, so it runs after the build stage.<br>
`tags`: Like the build-job, this job also requires a runner with the ci/cd tag.<br>
`script`: The job runs a simple echo command, indicating that it is running some tests (though it doesn’t specify exactly what it is testing).<br>
### 4. Test Job 2 (test-job2)
```bash
test-job2:
  stage: test
  tags:
    - ci/cd
  script:
    - echo "This job tests something, but takes more time than test-job1."
    - sleep 20
```
`stage: test`: This job is also part of the test stage and runs after test-job1.<br>
`tags`: It also runs on a runner with the ci/cd tag.<br>
`script`: This job runs a two-step process:<br>
It echoes a message indicating that this test job takes more time than test-job1.<br>
It sleeps for 20 seconds, simulating a test that takes longer to execute. This introduces a delay in the pipeline for this particular job.<br>
### 5. Deploy Job (deploy-prod)
```bash
deploy-prod:
  stage: deploy
  tags:
    - ci/cd
  script:
    - echo "Deploying to production"
```
`stage`: deploy: This job is part of the deploy stage and runs after all jobs in the test stage have completed.<br>
`tags`: It requires a runner with the ci/cd tag, just like the other jobs.<br>
`script`: This job performs the deployment to the production environment (though the actual deployment commands are not specified here; it simply echoes a message stating the deployment is happening).<br>
#
#### Overall Process
`Build`: First stage, contains a job (build-job) that greets the GitLab user.<br>
`Test`: Second stage, contains two test jobs (test-job1 and test-job2), which test something (with test-job2 taking longer to run).<br>
`Deploy`: Final stage, with a job (deploy-prod) that indicates deployment to production.<br>
- ##### The pipeline runs sequentially from build → test → deploy, with each job executing on a runner tagged with ci/cd.
#
### Create a Gitlab Runner
- Tags
`ci/cd`: The runner has been tagged with ci/cd. This means it is intended to execute jobs that are specifically tagged with ci/cd in the GitLab CI/CD pipeline configuration. You might configure jobs in your .gitlab-ci.yml file to run only on runners with this tag.<br>
#### 1. Open GitLab and Select Your Project
- Go to your GitLab instance.
- In the left sidebar, click on the project you want to configure the runner for.
#### 2. Navigate to CI/CD Settings
- In your project, go to the left sidebar and click on Settings.
- Under the settings menu, click on CI / CD (or you can search for CI/CD settings).
#### 3. Go to the Runners Settings
- Scroll down or expand the Runners section.
- Click on the Expand button if necessary to view more options about runners.
#### 4. Create a New Runner
- Add a New Runner by clicking on "Set up a specific runner" or the "New runner" button.

#### Configure Runner Settings:

- Tag: Enter a tag for your runner (e.g., ci/cd).
- Maximum Job Timeout: Set the job timeout to 13m 20s (or adjust as necessary).
- Protected: Ensure that the runner is marked as protected to restrict it to protected branches or tags only.
- Click "Create Runner" to proceed.

#### 5. Navigate to the New Runner Page
-After clicking Create Runner, GitLab will navigate you to a page with specific instructions for setting up the runner on your machine.

#### 6. Ensure GitLab Runner is Installed on Linux
- On the new page, it will ask you to select the operating system for the runner.

#### Select Linux.
- Before proceeding with the setup commands, ensure GitLab Runner is installed on the Linux machine where you will run the commands. If it is not installed, follow these steps:
####  Install GitLab Runner
```bash
# Download and install GitLab Runner package
sudo apt-get update
sudo apt-get install -y curl
curl -L --output /tmp/gitlab-runner.deb https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_amd64.deb
sudo dpkg -i /tmp/gitlab-runner.deb
```
#### Verify the Installation
```bash
gitlab-runner --version
```
#### Register the GitLab Runner
```bash
gitlab-runner register --url https://gitlab.com --token glrt-************
```
###### This command will prompt you to configure the runner:
- GitLab instance URL: 
- Registration token: 
- Runner description: 
- Runner tags: 
- Executor:
#### Start the GitLab Runner
```bash
sudo gitlab-runner start
```
### After completing this process you can see the message Runner created sucessfully in the Gitlab Runner.
