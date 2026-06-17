.gitlab-ci.yml
201 B
include:
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'templates/microservices/gradle.java.yml'
variables:
  RELEASE_NAME: javasimulator
  CHART_NAME: javautilityapisimulator

  plugins {
    id 'java'
    id 'org.springframework.boot' version "${springboot}"
    id 'io.spring.dependency-management' version "${spring_plugin}"
}

group = 'com.epay.simulator'
version = "${version}"

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

repositories {
    flatDir {
        dirs 'libs'
    }
    mavenLocal()
    maven {
        url "https://gitlab.epay.sbi/api/v4/projects/16/packages/maven"
        credentials(PasswordCredentials) {
            username = project.findProperty("gitlab.username") ?: System.getenv("CI_USERNAME")
            password = project.findProperty("gitlab.token") ?: System.getenv("CI_JOB_TOKEN")
        }
        authentication {
            basic(BasicAuthentication)
        }
    }
    maven {//SBI JAVA SDK Package Registry to download in development java SDK.
        url 'https://gitlab.epay.sbi/api/v4/projects/8/packages/maven'
        credentials(PasswordCredentials) {
            username = project.findProperty("gitlab.username") ?: System.getenv("CI_USERNAME")
            password = project.findProperty('gitlab_sdk_token') ?: System.getenv("CI_JOB_TOKEN")
        }
        authentication {
            basic(BasicAuthentication)
        }
    }
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-mail'
    implementation 'org.springframework.boot:spring-boot-starter-aop'
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation "org.hibernate.orm:hibernate-envers"
    implementation "javax.persistence:javax.persistence-api:${jakarta_persistence}"
    implementation "commons-io:commons-io:${commons_io}"
    implementation "org.apache.commons:commons-lang3:${commons_lang3}"
    implementation "org.modelmapper:modelmapper:${model_mapper}"
    implementation "com.fasterxml.jackson.datatype:jackson-datatype-joda:${jackson_datatype}"

    implementation "org.springframework:spring-context-support:${spring_context}"
    implementation "javax.mail:javax.mail-api:${javax_mail}"
    implementation "com.sun.mail:javax.mail:${javax_mail}"
    implementation "org.thymeleaf:thymeleaf:${thymeleaf}"
    implementation "org.thymeleaf:thymeleaf-spring5:${thymeleaf_spring5}"
    implementation "org.apache.commons:commons-text:${commons_txt}"

    //Utility
    implementation "com.sbi.epay:logging-service:${sbi_logging}"
    implementation "com.sbi.epay:encryption-decryption-service:${sbi_crypto}"
    implementation "com.sbi.epay:authentication-service:${sbi_auth}"
    implementation "com.sbi.epay:notification-service:${sbi_notification}"
    implementation "com.sbi.epay:captcha-service:${sbi_captcha}"
    implementation "com.jhlabs:filters:${jhlabs}"
    implementation "net.sf.sociaal:freetts:${freeTTS}"
    runtimeOnly "com.h2database:h2:${h2_driver}"
    //runtimeOnly "com.oracle.database.jdbc:ojdbc11:${oracle_driver}"
    implementation "in.bank.sbi.sbiepay:sbiepay_java_sdk:${java_sdk}"

    //S3
    implementation "software.amazon.awssdk:s3:${aws_s3}"
    implementation "software.amazon.awssdk:auth:${aws_s3}"
    implementation "software.amazon.awssdk:apache-client:${aws_s3}"

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework.security:spring-security-test'
    implementation "org.springdoc:springdoc-openapi-starter-webmvc-ui:${swagger}"
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'

    //Just for Testing
    runtimeOnly "io.jsonwebtoken:jjwt-jackson:${jjwt}"
    implementation "io.jsonwebtoken:jjwt-api:${jjwt}"
    runtimeOnly "io.jsonwebtoken:jjwt-impl:${jjwt}"

    // Lombok
    compileOnly "org.projectlombok:lombok:${lombok}"
    annotationProcessor "org.projectlombok:lombok:${lombok}"

    //sftp - Client
    implementation "com.jcraft:jsch:${sftp_client}"
    //sftp - Server
    implementation "org.apache.sshd:sshd-core:${sftp_server}"
    implementation "org.apache.sshd:sshd-sftp:${sftp_server}"

    implementation 'org.springframework.retry:spring-retry'

}

springBoot  {
    buildInfo()
}


gradle.java.yml
10.94 KiB
include:
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/build/gradle.java.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/test/gradle.java.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/sast/sast.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/sast/fortify.sast.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/release/version.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/release/release.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/build/podman.container.yml'
  - project: 'epay/devops/ci-templates'
    ref: main
    file: 'ci/deploy/deployment.yml'
stages:
- build
- test
- sast
- hp-fortify
- release
- dockerbuild
- deploy
variables:
  CI_TEMPLATE_REGISTRY_HOST: "registry.dev.sbiepay.sbi:8443"
  CI_TEMPLATE_REGISTRY_HOST_PREPROD: "epaynonprod-registry-quay-quay-enterprise.apps.preprod.epay.sbi"
  CI_TEMPLATE_REGISTRY_HOST_PREPRODDC: "dc-preprod-registry-quay-quay-enterprise.apps.dcpreprod.epay.sbi"
  CI_TEMPLATE_REGISTRY_HOST_PROD: "registry.prod.epay.sbi:8443"
  CI_TEMPLATE_REGISTRY_HOST_PRODDR: "epayproddr-registry-quay-quay-enterprise.apps.dr.prod.epay.sbi"
  CI_TEMPLATE_REGISTRY_HOST_PRODDC: "proddc-registry-route-quay-enterprise.apps.dc.prod.epay.sbi"
  GIT_STRATEGY: clone
  CI_USERNAME: "gitlab-ci-token"
  RELEASE_NAME: null
  CHART_NAME: null
  IMAGE_NAME: "library/$CHART_NAME"
.rules:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(main)|(develop)|(release)(\/.+)*$/'
    - if: '$CI_COMMIT_BRANCH =~ /^(main)|(develop)|(release)(\/.+)*$/'
    - when: never
     
build:
  extends: 
    - .build
    - .rules
test:
  extends: 
    - .test
    - .rules
sast:
  extends:
    - .sast
    - .rules
hp-fortify:
  extends:
    - .HPFortify
    - .rules
tag:
  extends:
    - .tag
dockerbuild:
  extends:
    - .dockerbuild
    - .rules
deploy-dev:
  variables:
    ENV: dev
    SERVICE_NAME: $CI_PROJECT_NAME
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(develop)$/
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(develop)$/
      when: manual
      allow_failure: true
    - when: never
deploy-sit:
  variables:
    ENV: sit
    SERVICE_NAME: $CI_PROJECT_NAME
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
deploy-uat:
  variables:
    ENV: uat
    SERVICE_NAME: $CI_PROJECT_NAME
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
deploy-int:
  variables:
    ENV: int
    SERVICE_NAME: $CI_PROJECT_NAME
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
promote-image-pre-prod:
  stage: deploy
  variables:
    REGISTRY_USERNAME: $PREPROD_REGISTRY_USERNAME
    REGISTRY_PASSWORD: $PREPROD_REGISTRY_PASSWORD
    TARGET_IMAGE_NAME: "microservices/$CHART_NAME"
  script:
    - podman login -u $IMAGE_REGISTRY_USERNAME -p $IMAGE_REGISTRY_PASS $CI_TEMPLATE_REGISTRY_HOST --tls-verify=false
    - podman login -u microservices+preprod -p Y37PK8H33NIJVND7HA68OJQZX776SKHPP81XI7BQ94JH92UAJ1GVA144GPYC9PGU epaynonprod-registry-quay-quay-enterprise.apps.preprod.epay.sbi --tls-verify=false
    #- podman login -u $REGISTRY_USERNAME -p $REGISTRY_PASSWORD $CI_TEMPLATE_REGISTRY_HOST_PREPROD --tls-verify=false
    - podman pull $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION --tls-verify=false
    - podman tag $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION $CI_TEMPLATE_REGISTRY_HOST_PREPROD/$TARGET_IMAGE_NAME:$VERSION
    - podman push $CI_TEMPLATE_REGISTRY_HOST_PREPROD/$TARGET_IMAGE_NAME:$VERSION --tls-verify=false
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
deploy-pre-prod:
  variables:
    ENV: pre-prod
    SERVICE_NAME: $CI_PROJECT_NAME
    DEPLOYMENT_BRANCH: main
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
deploy-perf:
  variables:
    ENV: perf
    SERVICE_NAME: $CI_PROJECT_NAME
    DEPLOYMENT_BRANCH: main
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
promote-image-pre-prod-dc:
  stage: deploy
  variables:
    REGISTRY_USERNAME: $PREPROD_DC_REGISTRY_USERNAME
    REGISTRY_PASSWORD: $PREPROD_DC_REGISTRY_PASSWORD
    TARGET_IMAGE_NAME: "microservices/$CHART_NAME"
  script:
    - podman login -u $IMAGE_REGISTRY_USERNAME -p $IMAGE_REGISTRY_PASS $CI_TEMPLATE_REGISTRY_HOST --tls-verify=false
 #   - podman login -u $REGISTRY_USERNAME -p $REGISTRY_PASSWORD $CI_TEMPLATE_REGISTRY_HOST_PREPRODDC --tls-verify=false
 #   - podman login -u='microservices+microservices' -p='JO3ID26271METLG482AEBXI2BJPNPRLJEN9G6BA0CIU1IW3IOGGAPKQVMPLW5N98' dc-preprod-registry-quay-quay-enterprise.apps.dcpreprod.epay.sbi --tls-verify=false
    - podman login -u='microservices+dcpreprod' -p='B9C8N5CY16BTJR8G5D5H09PSFTV35MLS2LPZYE452WGBR1WM3VLMFLM40IGWZYFI' dc-preprod-registry-quay-quay-enterprise.apps.dcpreprod.epay.sbi --tls-verify=false
    - podman pull $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION --tls-verify=false
    - podman tag $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION $CI_TEMPLATE_REGISTRY_HOST_PREPRODDC/$TARGET_IMAGE_NAME:$VERSION
    - podman push $CI_TEMPLATE_REGISTRY_HOST_PREPRODDC/$TARGET_IMAGE_NAME:$VERSION --tls-verify=false
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
deploy-pre-prod-dc:
  variables:
    ENV: pre-prod-dc
    SERVICE_NAME: $CI_PROJECT_NAME
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(release)(\/.+)*$/
      when: manual
      allow_failure: true
    - when: never
promote-image-prod-dr:
  stage: deploy
  variables:
    REGISTRY_USERNAME: $PROD_DR_REGISTRY_USERNAME
    REGISTRY_PASSWORD: $PROD_DR_REGISTRY_PASSWORD
    TARGET_IMAGE_NAME: "microservices/$CHART_NAME"
  script:
    - podman login -u $IMAGE_REGISTRY_USERNAME -p $IMAGE_REGISTRY_PASS $CI_TEMPLATE_REGISTRY_HOST --tls-verify=false
    #- podman login -u $REGISTRY_USERNAME -p $REGISTRY_PASSWORD $CI_TEMPLATE_REGISTRY_HOST_PRODDR --tls-verify=false
    - podman login -u='microservices+microservices_prod_dr' -p='DC91U2U38YMH21BLBXYTY8JLNNYXH0GZT7LQ6B4261IR5LJPEXAE7EGRC76866XW' epayproddr-registry-quay-quay-enterprise.apps.dr.prod.epay.sbi --tls-verify=false
    - podman pull $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION --tls-verify=false
    - podman tag $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION $CI_TEMPLATE_REGISTRY_HOST_PRODDR/$TARGET_IMAGE_NAME:$VERSION
    - podman push $CI_TEMPLATE_REGISTRY_HOST_PRODDR/$TARGET_IMAGE_NAME:$VERSION --tls-verify=false
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(main)$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(main)$/
      when: manual
      allow_failure: true
    - when: never
deploy-prod-dr:
  variables:
    ENV: prod-dr
    SERVICE_NAME: $CI_PROJECT_NAME
    DEPLOYMENT_BRANCH: main
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(main)$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(main)$/
      when: manual
      allow_failure: true
    - when: never
promote-image-prod-dc:
  stage: deploy
  variables:
    REGISTRY_USERNAME: $PROD_DC_REGISTRY_USERNAME
    REGISTRY_PASSWORD: $PROD_DC_REGISTRY_PASSWORD
    TARGET_IMAGE_NAME: "microservices/$CHART_NAME"
  script:
    - podman login -u $IMAGE_REGISTRY_USERNAME -p $IMAGE_REGISTRY_PASS $CI_TEMPLATE_REGISTRY_HOST --tls-verify=false
    - echo "$REGISTRY_USERNAME:$REGISTRY_PASSWORD"
    - podman login -u='microservices+microservices' -p='VY62CTW20B8V2TGIO0WNZGAZXDQCZUGN5F83VHY2ORR4MJRKRPO4DAPFIZ97LRYR' proddc-registry-route-quay-enterprise.apps.dc.prod.epay.sbi --tls-verify=false
    - podman pull $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION --tls-verify=false
    - podman tag $CI_TEMPLATE_REGISTRY_HOST/$IMAGE_NAME:$VERSION $CI_TEMPLATE_REGISTRY_HOST_PRODDC/$TARGET_IMAGE_NAME:$VERSION
    - podman push $CI_TEMPLATE_REGISTRY_HOST_PRODDC/$TARGET_IMAGE_NAME:$VERSION --tls-verify=false
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(main)$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(main)$/
      when: manual
      allow_failure: true
    - when: never
deploy-prod-dc:
  variables:
    ENV: prod-dc
    SERVICE_NAME: $CI_PROJECT_NAME
    DEPLOYMENT_BRANCH: main
    VERSION: $VERSION
  extends:
    - .trigger_cd
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(main)$/
      when: manual
      allow_failure: true
    - if: $CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME =~ /^(main)$/
      when: manual
      allow_failure: true
    - when: never
