pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        
        stage('Build') {
            steps {
                sh 'echo "Building Flask application..."'
                sh 'python -m py_compile app.py'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'echo "Deploying application..."'
                sh 'docker build -t flask-jenkins-app .'
                sh 'docker run -d -p 5000:5000 flask-jenkins-app'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
