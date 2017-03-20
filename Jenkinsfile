#!/usr/bin/groovy

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '7', artifactNumToKeepStr: '10', daysToKeepStr: '', numToKeepStr: '')), pipelineTriggers([])])

def currentBranch = ""
def currentVersion = ""

node {

    timeout(20) {
      stage('Checkout') {
        checkout scm
      }
      
      stage('Build') {
        sh 'mvn clean deploy'
      }
      
      stage('Deploy') {
        print("TODO")
      }
    }
}
