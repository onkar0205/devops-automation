pipeline{
    agent any
    tools {
        maven 'Maven3'
    }
    stages{
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/onkar0205/devops-automation.git']]])
            }
        }
        stage('sonar-analysis'){
            steps{
                sh "mvn clean verify sonar:sonar \
                  -Dsonar.projectKey=Devops-Automation \
                  -Dsonar.projectName='Devops-Automation' \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.token=sqp_5700e16b7ede19c281acf4462d16f19353400ba3"
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
        stage('Deploy Docker Container') {
            steps {
                script {
                    sh 'docker run -d --name devops-integration-container -p 7081:8080 naikonkar0205/devops-integration'
                }
            }
        }
    }
}
