[osuser@bastion ~]$ oc get cm comm-config -o yaml
apiVersion: v1
data:
  application.yml: |
    server:
      servlet:
        context-path: /api/communication/v1
      port: 9098

    spring:
      application:
        name: epay_communication_service
      profiles:
        active: ""

      datasource:
        driver-class-name: oracle.jdbc.OracleDriver
        url: jdbc:oracle:thin:@10.177.135.4:1590:epaysit2
        username: EPAYCOMM
        password: Lkjh_123

      jpa:
        properties:
          hibernate:
            jdbc:
              batch_size: 1000
              order_updates: true
              order_inserts: true
              batch_versioned_data: true
            show_sql: true
            format_sql: true
        show-sql: true

      liquibase:
        change-log: classpath:db/changelog/db.changelog-master.xml
        enabled: true
        drop-first: false

      kafka:
        bootstrapServers: sit-kafka-cluster-kafka-bootstrap.sit-kafka.svc.cluster.local:9092

        properties:
          security.protocol: PLAINTEXT
          # ssl:
          #   truststore:
          #     location: /certs/kafka/cluster-ca.p12
          #     password: pbUnnCLwMfNm
          #     type: PKCS12
          #   keystore:
          #     location: /certs/kafka/client-ca.p12
          #     password: MvgumtgtCJiq
          #     type: PKCS12

        consumer:
          groupId: communication-consumers
          keyDeserializer: org.apache.kafka.common.serialization.StringDeserializer
          valueDeserializer: org.apache.kafka.common.serialization.StringDeserializer
          autoOffsetReset: latest
          enableAutoCommit: true
          autoCommitInterval: 100
          sessionTimeoutMS: 300000
          requestTimeoutMS: 420000
          fetchMaxWaitMS: 200
          maxPollRecords: 5
          retryMaxAttempts: 3
          retryBackOffInitialIntervalMS: 10000
          retryBackOffMaxIntervalMS: 30000
          retryBackOffMultiplier: 2
          spring.json.trusted.packages: com.epay.communication
          numberOfConsumers: 1

        producer:
          keyDeserializer: org.apache.kafka.common.serialization.StringSerializer
          valueDeserializer: org.apache.kafka.common.serialization.StringSerializer
          acks: all
          retries: 3
          batchSize: 1000
          lingerMs: 1
          bufferMemory: 33554432

        topic:
          partitions: 4
          replicationFactor: 1
          gstInvoiceDownload: gst_invoice_download_topic
          gstReportStatusTopic: gst_report_status_topic

    security:
      cors:
        allowed:
      #health endpoint will not require the Origin header.
          endpoints: /actuator/health, /actuator/health/readiness, /actuator/health/liveness, /actuator/masterdata/refresh/*, /sftp/**
          origins: "*"
        origin: "*"
      jwt:
        secret:
          issuer: sbi.epay
          key: bsrfgskjfhsdjkhkflkdlksdlfkskfwperip3ke3le3lmldrnkfnhiewjfejfokepfkldkfoikfokork3dklwedlsvflvkfkvlkdfvodkvcdokro3
      whitelist:
        urls: /webjars/, /actuator/health, /swagger-resources/, /v3/api-docs/**, /swagger-ui/**, /index.html, /login, /consent/**, /testconsent/**

    servlet:
      multipart:
        max-file-size: 50MB
        max-request-size: 50MB

    external:
      #External Base Path
      api:
        eis:
          services:
            source:
              id: EY
              destination: CRM
            privateKey:
              path: /certs/SBI_private.key
            publicKey:
              path: /certs/EIS_ENC_UAT.cer
        services:
          endpoint:
            consentEis: https://eissiuat.sbi.co.in/gen6/gateway


    scheduled:
      gstn:
        monitor:
          time: 0 */30 * * * *
      ekuber:
        cron: 0 */1 * * * *
      consent:
        cron: 0 * * * * *


    queue:
      threadPool:
        size:
          core: 5
          max: 10
          batch: 1000

    sftp:
      host: localhost
      port: 2222
      username: root
      password: root
      session:
        timeout: 30000
      dir: /home/default/COMM
      gstIn:
        baseDir: tmp/
        file:
          upload: upload
          success: success
          error: error
          archive: archive

    aws:
      s3:
        url: https://s3store.bank.sbi
        region: ap-south-1
        accessKey: IDHJO1513FFMMNLPR9BC
        secretKey: avXqEPv5B_QqqPK0D1VJzP3pSyAA4x31Zu_KucQ9
        bucket: epay-nonprod-s3bucket

    gst:
      report:
        retry:
          max: 3
          delay: 3000
        encryption:
          key: key/EPY_20190528_20190528_PRI.key

    logging:
      level:
        liquibase: INFO
        com.epay.sbi: INFO
        com.epay.cs: INFO
        org.apache.kafka: ERROR
    max:
      gstn:
        allowed: 10
kind: ConfigMap
metadata:
  annotations:
    meta.helm.sh/release-name: comm
    meta.helm.sh/release-namespace: sit-communication
  creationTimestamp: "2026-03-10T05:42:40Z"
  labels:
    app.kubernetes.io/instance: comm
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: commsservice
    app.kubernetes.io/version: 1.16.0
    helm.sh/chart: commsservice-0.1.0
  name: comm-config
  namespace: sit-communication
  resourceVersion: "823865892"
  uid: 5ec58d59-98b6-4239-a884-25a2507b9222
[osuser@bastion ~]$
