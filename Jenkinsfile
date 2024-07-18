pipeline {
    agent any
    tools {
        maven 'Maven3'
        sonarScanner 'SonarScanner for Maven'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/onkar0205/devops-automation.git']]])
            }
        }
        stage('Build and Test') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            def mvn = tool 'Default Maven';
            withSonarQubeEnv() {
                sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Devops-Automation -Dsonar.projectName='Devops-Automation'"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t naikonkar0205/devops-integration .'
                }
            }
        }
        stage('Push Image to Hub') {
            steps {
                script {
                    sh 'docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}'
                    sh 'docker push naikonkar0205/devops-integration'
                }
            }
        }
    }
}
