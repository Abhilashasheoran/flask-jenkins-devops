pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flaskapp'
        CONTAINER_NAME = 'flaskapp'
        PORT = '5000'
    }

    stages {
        stage('📥 Clone Repository') {
            steps {
                echo '🔄 Cloning Git repository...'
                git 'https://github.com/Abhilashasheoran/flask-jenkins-devops.git'
            }
        }

        stage('📦 Install Python Dependencies') {
            steps {
                echo '📦 Installing Python dependencies...'
                sh 'pip3 install --user -r requirements.txt'
            }
        }

        stage('✅ Run Unit Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'python3 -m pytest'
            }
        }

        stage('🧽 Clean Old Docker Resources') {
            steps {
                echo '🧹 Stopping and removing any existing container...'
                // Remove 'sudo' to avoid terminal password issues
                sh 'podman stop ${CONTAINER_NAME} || true'
                sh 'podman rm ${CONTAINER_NAME} || true'
            }
        }

        stage('🐳 Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'podman build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('🚀 Run Docker Container') {
            steps {
                echo '🚀 Running Docker container...'
                sh 'podman run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}:latest'
            }
        }
    }

    post {
        failure {
            echo '❌ Build/Deployment Failed. Check Console Output.'
        }
    }
}
