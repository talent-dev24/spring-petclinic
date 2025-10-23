pipeline {
  agent any 
  options { 
    buildDiscarder(logRotator(numToKeepStr: '10')) 
    timestamps()
    }

  stages{
    stage("git clone"){
      step{
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenking-github-ssh-key', url: 'git@github.com:talent-dev24/spring-petclinic.git']])
      }
    }
    stage("maven build"){
      step{
        sh 'mvn clean package'
      }
    }
    stage("junit report"){
      step{
        junit 'target/surefire-reports/*.xml'
      }
    }
    stage("archive artifact"){
      step{
        archive 'target/*.jar'
      }
    }
    stage("nexus upload"){
      step{
        nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/*.jar', type: 'jar']], credentialsId: 'nexus-cred-id', groupId: 'org.springframework.samples', nexusUrl: '172.25.213.228:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'petclinic-pipeline', version: '3.5.0-SNAPSHOT'
      }
    }
  }
}
