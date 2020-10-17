
Declarative pipeline

pipeline {
  agent { label 'node-1' }
  stages {
    stage('Source') {
      steps {
        git 'https://github.com/digitalvarys/jenkins-tutorials.git''
      }
    }
    stage('Compile') {
      tools {
        gradle 'gradle4'
      }
      steps {
        sh 'gradle clean compileJava test'
      }
    }
  }
}



checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'd9812c0d-9b10-4ddc-bc79-1c83e9b10c5b', url: 'git@ssh.dev.azure.com:v3/deep16134/demo/mvnrepo']]])  }

Scripted pipeline
node('node1') {
    stage "Create build output"
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '090b8705-028c-41d8-b4e7-9e8ca6fe8c86', url: 'git@ssh.dev.azure.com:v3/deep16134/demo/mvnrepo']]])
    sh "mvn clean package"
    // Make the output directory.
    sh "mkdir -p output"

    // Write an useful file, which is needed to be archived.
    writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

    // Write an useless file, which is not needed to be archived.
    writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

    stage "Archive build output"
    
    // Archive the build output artifacts.
    archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
}


sonar.jdbc.username=sonarqube
sonar.jdbc.password=Pass@123456

sonar.web.javaAdditionalOpts=-server
sonar.web.host=3.15.217.3

server {
    listen 80;
    server_name sonarqube.example.com;

    location / {
        proxy_pass http://3.15.217.3:9000;
    }
}


S3-bucket

node {
    stage "Create build output"
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '090b8705-028c-41d8-b4e7-9e8ca6fe8c86', url: 'git@ssh.dev.azure.com:v3/deep16134/demo/mvnrepo']]])
    stage "Build"
    sh "mvn clean package"
    stage "s3"
    sh " aws s3 cp /var/lib/jenkins/workspace/asdf/target/*jar s3://vasu2"
    }
