pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/22120139/ci-cd-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nzhuy1404/ci-cd-project:latest .' // Đổi "nzhuy1404" thành username Docker Hub của bạn
            }
        }
	stage('Clean Up Old Container') {
            steps {
                script {
                    // Dừng container đang chạy (nếu có)
                    sh '''
                    docker ps -q --filter "name=ci-cd-container" | grep -q . && docker stop ci-cd-container || true
                    docker ps -a -q --filter "name=ci-cd-container" | grep -q . && docker rm ci-cd-container || true
                    '''
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withDockerRegistry([ credentialsId: 'dockerhub-credentials', url: '' ]) {
                    sh 'docker push nzhuy1404/ci-cd-project:latest'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8081:80 nzhuy1404/ci-cd-project:latest'
            }
        }
    }
}

