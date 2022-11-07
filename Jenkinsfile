pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t image-builder --target builder .'
            }
        }
        stage('Delivery stage') {
            steps {
                sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t kelmane/todo-be:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup') {
            steps {
                echo "Cleanup-stage"
                sh 'docker system prune -f'
            }
        }
        stage('push') {
            environment {
                dockerpwd = credentials('docker_pwd')
            }
            steps {
                    sh 'docker login -u kelmane -p ${dockerpwd}'
                    sh "docker push kelmane/todo-be:jenkins-$BUILD_NUMBER"
        }
    }
}
}
