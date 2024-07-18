pipeline {
    agent any
    tools{
        maven 'Maven3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/onkar0205/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            def mvn = tool 'Default Maven';
            withSonarQubeEnv() {
                sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Devops-Automation -Dsonar.projectName='Devops-Automation'"
            }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t naikonkar0205/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   sh 'docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}'
                   sh 'docker push naikonkar0205/devops-integration'
                    }
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}