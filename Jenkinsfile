pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        BRANCH_NAME = "${env.GIT_BRANCH ?: 'unknown'}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code from ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                // Replace with actual build commands, e.g., Maven or Gradle
                sh './mvnw clean install' // or: mvn clean install
            }
        }

        stage('Test') {
            steps {
                echo "Running tests on branch: ${env.BRANCH_NAME}"
                // Example test command
                sh './mvnw test'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
