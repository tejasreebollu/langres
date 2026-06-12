pipeline {
    agent any

    environment {
        IMAGE_NAME = "fraud-agent"
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_USER = "your-dockerhub-username"
        KUBE_DEPLOYMENT = "fraud-agent"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Installing dependencies..."
                # Example for Python project
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                echo "Running tests..."
                # Replace with your test command
                pytest -v || exit 1
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building Docker image..."
                docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                echo "Deploying to Kubernetes..."

                kubectl set image deployment/$KUBE_DEPLOYMENT \
                $KUBE_DEPLOYMENT=$DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG

                kubectl rollout status deployment/$KUBE_DEPLOYMENT
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                echo "Checking service health..."
                curl -f http://localhost:8080/health || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }

        failure {
            echo "❌ Deployment Failed! Rolling back..."
            sh '''
            kubectl rollout undo deployment/$KUBE_DEPLOYMENT
            '''
        }
    }
}
