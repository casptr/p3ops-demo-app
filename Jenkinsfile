
pipeline {

    agent none

    stages {
        stage('Build & Test app on Dotnet SDK image') {

            agent {
                dockerfile {
                    filename 'Dockerfile.dev'
                    dir 'docker'
                    args '-u root'
                }
            }

            stages {
               stage('Build app') {
                   steps {
                        echo 'Building app'
                        sh 'dotnet restore src/Server/Server.csproj'
                        sh 'dotnet build src/Server/Server.csproj'
                    }
               }
               stage('Test app') {
                   steps {
                        echo 'Running tests on app'
                        sh 'dotnet test tests/Domain.Tests/Domain.Tests.csproj'
                    }
               }
            }
        }
        stage('Build & Push app on Dotnet runtime image') {

            agent any

            environment {
                DOCKERHUB_CRED = credentials('dockerHub')
            }

            stages {
                stage('Build app production image') {
                    steps {
                        echo 'Building app Docker image'
                        sh 'docker compose build'
                    }
                }
                stage('Push to Docker Hub') {
                    steps {
                        echo 'Pushing image to Docker Hub'
                        sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
                        sh 'docker push csprt/p3ops-app:latest'
                    }
                }
            }
            post {
                always {
                    sh 'docker logout'
                }
            }
        }
    }
}
