pipeline {

    agent any

    environment {

        IMAGE_NAME="aa3000/mykoppee"
        IMAGE_TAG="${BUILD_NUMBER}"

    }

    stages {

        stage('Build Docker Image') {

            steps {

                bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'

            }
        }

        stage('Login DockerHub') {

            steps {

                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {

                    bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    '''
                }

            }
        }

        stage('Push Image') {

            steps {

                bat 'docker push %IMAGE_NAME%:%IMAGE_TAG%'

            }
        }

        stage('Deploy to Kubernetes') {

            steps {
                 bat '''
                 set KUBECONFIG=C:\\Users\\ALOK SINGH\\.kube\\config
                 kubectl apply -f deployment.yaml
                 kubectl apply -f service.yaml
                 '''
             }

        }

    }

}
