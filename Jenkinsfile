

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
                    sh 'docker run -d --name devops-integration-container -p 8080:8080 naikonkar0205/devops-integration'
                }
            }
        }
    }
}


import pyodbc
server = 'prosyncommonoss001.sql.azuresynapse.net'
database = 'prosyndwoss'
username = 'heimdall_user'
password = '{S3fUV29=u^~L}'
driver= '{ODBC Driver 17 for SQL Server}'

with pyodbc.connect('DRIVER='+driver+';SERVER=tcp:'+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password) as conn:
    with conn.cursor() as cursor:
        cursor.execute("SELECT TOP 3 name, collation_name FROM sys.ugg_pm_metrics_junos_space_aggregated")
        row = cursor.fetchone()
        while row:
            print (str(row[0]) + " " + str(row[1]))
            row = cursor.fetchone()