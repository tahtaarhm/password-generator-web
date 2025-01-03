pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning repository..."
                    // Clone the main branch
                    git branch: 'main', url: 'https://github.com/tahtaarhm/password-generator-web.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for main branch..."
                    bat "docker build -t password-generator-image-main ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    def branchPort = 8081 // Fixed port for the main branch

                    echo "Stopping and removing any existing container for main branch..."
                    bat "docker stop password-generator-container-main || exit 0"
                    bat "docker rm password-generator-container-main || exit 0"

                    echo "Running new container for main branch on port ${branchPort}..."
                    bat "docker run --rm -d --name password-generator-container-main -p ${branchPort}:80 password-generator-image-main"

                    echo "Container for main branch is running on port ${branchPort}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        always {
            echo 'Pipeline completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
