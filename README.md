Running with gitlab-runner 17.9.2 (14c5775c)
  on dc_runner_55 Wz1c0kGRX, system ID: s_300da9d7588c
Resolving secrets
Preparing the "docker" executor
00:01
Using Docker executor with image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle:1.0.0 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Authenticating with credentials from $DOCKER_AUTH_CONFIG
Pulling docker image artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 ...
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Authenticating with credentials from $DOCKER_AUTH_CONFIG
Pulling docker image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle:1.0.0 ...
Using docker image sha256:388b5e85519adde960bf31a199d19fcb2fb7abf2ebb10c26328d14802765695b for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle:1.0.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle@sha256:dc0868734113da7beffde7b6f2dbbb14696a3fc39200e1a308b370c0b3bf72de ...
Preparing environment
00:01
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Running on runner-wz1c0kgrx-project-2841-concurrent-0 via PE3DSOPGrunners4...
Getting source from Git repository
00:06
Fetching changes with git depth set to 20...
Initialized empty Git repository in /builds/itepaypg-sbiepay2/application/backend/epay_payment_service/.git/
Created fresh repository.
Checking out 629d5aa4 as detached HEAD (ref is release/1.2.0)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:08
Using docker image sha256:388b5e85519adde960bf31a199d19fcb2fb7abf2ebb10c26328d14802765695b for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle:1.0.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-virtual/custom-ci/epay-build-java-gradle@sha256:dc0868734113da7beffde7b6f2dbbb14696a3fc39200e1a308b370c0b3bf72de ...
$ java -version
openjdk version "21.0.11" 2026-04-21 LTS
OpenJDK Runtime Environment (Red_Hat-21.0.11.0.10-1) (build 21.0.11+10-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-21.0.11.0.10-1) (build 21.0.11+10-LTS, mixed mode, sharing)
$ gradle --version
Welcome to Gradle 9.4.1!
Here are the highlights of this release:
 - Java 26 support
 - Non-class-based JVM tests
 - Enhanced console progress bar
For more details see https://docs.gradle.org/9.4.1/release-notes.html
------------------------------------------------------------
Gradle 9.4.1
------------------------------------------------------------
Build time:    2026-03-19 08:46:28 UTC
Revision:      2d6327017519d23b96af35865dc997fcb544fb40
Kotlin:        2.3.0
Groovy:        4.0.29
Ant:           Apache Ant(TM) version 1.10.15 compiled on August 25 2024
Launcher JVM:  21.0.11 (Red Hat, Inc. 21.0.11+10-LTS)
Daemon JVM:    /usr/lib/jvm/java-21-openjdk-21.0.11.0.10-2.el9.x86_64 (no Daemon JVM specified, using current Java home)
OS:            Linux 4.18.0-553.126.1.el8_10.x86_64 amd64
$ mkdir -p "${CI_PROJECT_DIR}/.gradle/init.d"
$ cat > "${CI_PROJECT_DIR}/.gradle/init.d/epay-artifactory.init.gradle" << 'EOF' # collapsed multi-line command
$ cp "$JAVA_HOME/lib/security/cacerts" "$CACERT_PATH"
$ chmod 644 "$CACERT_PATH"
$ keytool -importcert -keystore "$CACERT_PATH" -storepass changeit -alias jfrog-internal -file "$JF_ARTIFACTORY_CERT" -noprompt
Certificate was added to keystore
$ export JAVA_TOOL_OPTIONS="-Djavax.net.ssl.trustStore=${CI_PROJECT_DIR}/cacerts-custom -Djavax.net.ssl.trustStorePassword=changeit"
$ keytool -list -keystore "$CACERT_PATH" -storepass changeit -alias jfrog-internal
Picked up JAVA_TOOL_OPTIONS: -Djavax.net.ssl.trustStore=/builds/itepaypg-sbiepay2/application/backend/epay_payment_service/cacerts-custom -Djavax.net.ssl.trustStorePassword=changeit
jfrog-internal, Jun 16, 2026, trustedCertEntry, 
Certificate fingerprint (SHA-256): 95:A7:28:EE:11:00:19:F7:94:F4:B5:F7:98:0A:D5:C6:17:5D:8A:6B:F6:97:46:8E:00:CC:CB:D2:56:35:5E:66
$ set -eo pipefail # collapsed multi-line command
Building VERSION: 1.2.0-rc.1
Picked up JAVA_TOOL_OPTIONS: -Djavax.net.ssl.trustStore=/builds/itepaypg-sbiepay2/application/backend/epay_payment_service/cacerts-custom -Djavax.net.ssl.trustStorePassword=changeit
To honour the JVM settings for this build a single-use Daemon process will be forked. For more on this, please refer to https://docs.gradle.org/9.4.1/userguide/gradle_daemon.html#sec:disabling_the_daemon in the Gradle documentation.
Daemon will be stopped at the end of the build 
[Incubating] Problems report is available at: file:///builds/itepaypg-sbiepay2/application/backend/epay_payment_service/build/reports/problems/problems-report.html
FAILURE: Build failed with an exception.
* Where:
Build file '/builds/itepaypg-sbiepay2/application/backend/epay_payment_service/build.gradle' line: 18
* What went wrong:
Could not compile build file '/builds/itepaypg-sbiepay2/application/backend/epay_payment_service/build.gradle'.
> startup failed:
  build file '/builds/itepaypg-sbiepay2/application/backend/epay_payment_service/build.gradle': 18: only buildscript {}, pluginManagement {} and other plugins {} script blocks are allowed before plugins {} blocks, no other statements are allowed
  
  For more information on the plugins {} block, please refer to https://docs.gradle.org/9.4.1/userguide/plugins_intermediate.html#sec:plugins_block in the Gradle documentation.
  
   @ line 18, column 1.
     plugins {
     ^
  
  1 error
* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights from a Build Scan (powered by Develocity).
> Get more help at https://help.gradle.org.
Deprecated Gradle features were used in this build, making it incompatible with Gradle 10.
You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.
For more on this, please refer to https://docs.gradle.org/9.4.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.
BUILD FAILED in 5s
Uploading artifacts for failed job
00:00
Uploading artifacts...
WARNING: build.env: no matching files. Ensure that the artifact path is relative to the working directory (/builds/itepaypg-sbiepay2/application/backend/epay_payment_service) 
ERROR: No files to upload                          
Cleaning up project directory and file based variables
00:01
ERROR: Job failed: exit code 1
