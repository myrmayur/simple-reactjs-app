pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS ='dockerhub'
        GITHUB_CREDENTIALS = 'github'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/myrmayur/simple-reactjs-app.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("myrmayur/react-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh 'docker rmi myrmayur/react-app:${env.BUILD_ID}'
                sh 'docker rmi myrmayur/react-app:latest'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
