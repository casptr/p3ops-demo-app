pipeline {

    agent any

    stages {
        stage('Verify') {
            steps {
                sh '''
                    docker version
                    docker compose version
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Test') {
            steps {
                sh 'docker container run p3ops-app:latest -d'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'HUB_USER', passwordVariable: 'HUB_TOKEN')]) {                      
                    sh '''
                        docker login -u $HUB_USER -p $HUB_TOKEN 
                        docker image tag p3ops-app:latest $HUB_USER/p3ops-app:latest
                        docker image push $HUB_USER/p3ops-app:latest
                    '''
                }
            }
        }
    }
}