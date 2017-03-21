#!groovy
@Library('jenkins2-pipeline-library') _

pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
      stage('Checkout') {
          steps {
            checkout scm
          }
      }
      
      stage('Build') {
           steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    mvn clean package
                '''
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                    step([$class: 'JacocoPublisher'])
                    archiveArtifacts '**/*.war'
                }
            }
      }
      stage('QA') {

          steps {
            withSonarQubeEnv('LocalSonar') {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    mvn sonar:sonar -Psonar
                '''
            }
        }
      }
      stage('Deploy') {
          steps {
            print("TODO")
          }
      }
    }
}
