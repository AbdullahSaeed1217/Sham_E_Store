pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Fetch code from Git repository
                git branch: 'main', url: 'https://github.com/your-username/your-django-repo.git'
            }
        }

        stage('Setup Environment') {
            steps {
                script {
                    // Use Python virtual environment
                    sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    source venv/bin/activate
                    python manage.py test
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image for Django
                    sh '''
                    docker build -t django-app:latest .
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker container
                    sh '''
                    docker run -d -p 8000:8000 --name django-app django-app:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup Docker containers and unused resources after pipeline
            sh '''
            docker stop django-app || true
            docker rm django-app || true
            docker system prune -f || true
            '''
        }
    }
}
