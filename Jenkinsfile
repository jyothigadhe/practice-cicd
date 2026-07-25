pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Build Stage'
            }
        }

        stage('Test') {
            steps {
                echo 'Test Stage'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t practice-cicd:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    docker login -u $USERNAME -p $PASSWORD
                    docker tag practice-cicd:latest gadhe/practice-cicd:latest
                    docker push gadhe/practice-cicd:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy Stage'
            }
        }
    }
}
