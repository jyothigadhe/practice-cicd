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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker tag practice-cicd:latest gadhe/practice-cicd:latest
                    docker push gadhe/practice-cicd:latest
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@98.130.129.170 << EOF
                    docker pull gadhe/practice-cicd:latest
                    docker stop practice-cicd || true
                    docker rm practice-cicd || true
                    docker run -d --name practice-cicd -p 80:80 gadhe/practice-cicd:latest
                    EOF
                    '''
                }
            }
        }
    }
}
