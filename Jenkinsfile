#!/usr/bin/env groovy

pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Testing the integration for jfeature/payment ..."
                }
            }
        }
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
        stage('notify') {
            steps {
                script {
                    echo "Sending notification to team..."
                }
            }
        }
    }
}
