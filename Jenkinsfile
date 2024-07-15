
pipeline {
    agent none

    stages {
        stage('Build & Test app') {
            agent {
                dockerfile {
                    filename 'Dockerfile.dev'
                    dir 'docker'
                    args '-u root'
                }
            }
            stages {
               stage('Build app on Dotnet SDK image') {
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
               stage('Publish app') {
                   steps {
                        echo 'Publishing app'
                        sh 'dotnet publish src/Server/Server.csproj -c Release -o publish'
                    }
               }
               post {
                   failure {
                        echo 'Failed'
                    }
               }
            }
        }
        stage('Build & Push app') {
            agent any
            stage('Build app on Dotnet runtime image') {
                steps {
                    sh 'docker compose build'
                }
            }
            stage('Push app') {
                steps {
                    echo 'Pushing app'
                }
            }
        }
    }
}



// pipeline {

//     agent none

//     stages {
//         stage('Build & Test app') {
//             agent {
//                 dockerfile {
//                     filename 'Dockerfile.dev'
//                     dir 'docker'
//                 }
//             }
//             stages {
//                 stage 'Build app' {
//                     steps {
//                         echo 'Building dev Docker container'
//                         sh 'ls'
//                     }
//                 }
//             }
//         }
//         stage('Push') {
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'HUB_USER', passwordVariable: 'HUB_TOKEN')]) {                      
//                     sh '''
//                         docker login -u $HUB_USER -p $HUB_TOKEN 
//                         docker image tag p3ops-app:latest $HUB_USER/p3ops-app:latest
//                         docker image push $HUB_USER/p3ops-app:latest
//                     '''
//                 }
//             }
//         }
//     }
// }