pipeline {

    agent { label 'selenium-agent' }

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        BUILD_ENV = "QA"
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                bat 'java -version'
                bat 'mvn -version'
            }
        }

        stage('Clean') {
            steps {
                bat 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                bat 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {

        success {
            echo 'Build Successful'
        }

        failure {
            echo 'Build Failed'
        }

        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}