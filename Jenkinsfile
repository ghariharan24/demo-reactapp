pipeline {
    agent any

    environment {
          DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        DOCKER_IMAGE = 'devopsnxt-image/react-app'
         REACT_APP_PATH = 'demo-reactapp' 
     }

    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/ghariharan24/demo-reactapp.git'
            }
        }

        stage('Build React App') {
            steps {
                script {
                    dir(REACT_APP_PATH) {
                        sh 'npm install'
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

         stage('Run Docker Image') {
            steps {
                script {
                    sh 'docker run -p 3000:3000 $(docker images --format=\'{{.ID}}\' | head -1)'
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'docker logout'
            }
        }
    }
}
