pipeline {
    agent any
    tools {
        maven 'Maven3'
        // sonarScanner 'SonarScanner for Maven' // Ensure this plugin is installed
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
        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('your-sonar-server-url') {
        //             sh 'mvn sonar:sonar'
        //         }
        //     }
        // }
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
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
