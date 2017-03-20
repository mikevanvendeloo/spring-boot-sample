#!/usr/bin/groovy
pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
      stage('Checkout') {
        checkout scm
      }
      
      stage('Build') {
           steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    mvn clean deploy
                '''
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
      }
      
      stage('Deploy') {
        print("TODO")
      }
    }
}
