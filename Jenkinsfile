pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapache-image'
    }

    stages {
        stage('Nettoyage du workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clonage du dépôt GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/KevGithubb/projet-Devops.git'
            }
        }

        stage('Création du Dockerfile') {
            steps {
                script {
                    writeFile file: 'Dockerfile', text: '''
FROM ubuntu:latest

RUN apt-get update && apt-get install -y \\
    apache2 \\
    net-tools \\
    iproute2 \\
    iputils-ping \\
    nano \\
    openssh-server \\
    && apt-get clean

COPY . /var/www/html/

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
'''
                }
            }
        }

        stage('Construction de l\'image Docker') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Vérification de l\'image') {
            steps {
                sh "docker images | grep ${IMAGE_NAME}"
            }
        }
    }
}
