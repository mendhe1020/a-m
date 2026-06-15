Running with gitlab-runner 17.9.2 (14c5775c)
  on dc_runner_54 zFOGPH9mk, system ID: s_300da9d7588c
Resolving secrets
Preparing the "docker" executor
00:01
Using Docker executor with image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using locally found image version due to "if-not-present" pull policy
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using locally found image version due to "if-not-present" pull policy
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
Preparing environment
00:01
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Running on runner-zfogph9mk-project-2838-concurrent-0 via PE3DSOPGrunners3...
Getting source from Git repository
00:09
Fetching changes...
Initialized empty Git repository in /builds/itepaypg-sbiepay2/infra/devops/deployment/.git/
Created fresh repository.
Checking out d92a4fed as detached HEAD (ref is main)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:01
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
$ git config --global http.sslVerify false
$ git config --global --add safe.directory '*'
$ git config --global user.name "ci"
$ git config --global user.email "ci.cedge@sbi.co.in"
$ git remote set-url origin "https://oauth2:$GITLAB_CI_DEPLOY_TOKEN@gitlab.sbi/$CI_PROJECT_PATH.git"
$ set -eo pipefail # collapsed multi-line command
========== DEBUG ==========
SERVICE_NAME=epaylite-bridge-txn-service
ENV=dev
VERSION=dev-fb09d97c
CHART_NAME=$CHART_NAME
RELEASE_NAME=epaybridgetxn
===========================
Updating dev/charts/epaylite-bridge-txn-service/values.yaml → image tag: dev-fb09d97c
SERVICE_PATH=dev/charts/epaylite-bridge-txn-service
ls: cannot access 'dev/charts/epaylite-bridge-txn-service': No such file or directory
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit code 1
