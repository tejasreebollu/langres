pipeline {
    agent any

    environment {
        IMAGE_NAME = "fraud-agent"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Source code is automatically checked out by Jenkins SCM 🚀"
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                // Example:
                // sh 'npm install'
                // sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests..."
                // Example:
                // sh 'npm test'
                // sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                echo "Docker push skipped (enable when credentials are added)"
                
                // Uncomment when DockerHub credentials are ready:
                /*
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
                */
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment skipped (Kubernetes not configured yet)"
                // kubectl apply -f k8s/
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
