pipeline {
    agent any

    stages {

        stage('Checkout App Code') {
            steps {
                dir('catalogue') {
                    git branch: 'main',
                        url: 'https://github.com/Sandeepkumar0088/azure-roboshop-catalogue.git'
                }
            }
        }

        stage('Build Image') {
            steps {
                dir('catalogue') {
                    sh 'docker build -t catalogue .'
                }
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag catalogue roboshop0088.azurecr.io/catalogue:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push roboshop0088.azurecr.io/catalogue:latest'
            }
        }

        stage('Checkout Helm Chart') {
            steps {
                dir('helm') {
                    git branch: 'main',
                        url: 'https://github.com/sdevops5427/azure-roboshop-catalogue.git'
                }
            }
        }
        stage('Get AKS Credentials') {
            steps {
                sh 'az aks get-credentials --resource-group cluster --name dev --overwrite-existing'
            }
        }

        stage('Deploy on AKS') {
            steps {
                dir('helm') {
                    sh 'helm upgrade -i catalogue . -f templates/catalogue.yml --install --take-ownership'
                }
            }
        }
    }
}
