#!groovy
@Library('jenkins2-pipeline-library') _

pipeline {
    agent any
    options {
        // Keep the 10 most recent builds
        buildDiscarder(logRotator(numToKeepStr:'10'))
    }
    triggers { pollSCM('H 4/* 0 0 1-5') }
    tools {
        maven 'M3'
    }

    def currentBranch
    stages {
      stage('Checkout') {
          steps {
            checkout scm
            echo gitCommitId()
            echo currentVersionString()
            echo extractPomVersion()
            currentBranch = gitBranch()
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
                    publishHTML target: [
                                allowMissing: false,
                                alwaysLinkToLastBuild: false,
                                keepAll: true,
                                reportDir: 'site/jacoco-ut',
                                reportFiles: 'index.html',
                                reportName: 'Unit Test Report'
                              ]
                     publishHTML target: [
                                allowMissing: false,
                                alwaysLinkToLastBuild: false,
                                keepAll: true,
                                reportDir: 'site/jacoco-it',
                                reportFiles: 'index.html',
                                reportName: 'Integration Test Report'
                              ]
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
            print("Build resultaat ${currentBuild.result}")
          }
          post {
            success {
                currentBuild.displayName="Succes ${currentVersion} #${env.BUILD_NUMBER}"
            }
          }
      }
    }
}
