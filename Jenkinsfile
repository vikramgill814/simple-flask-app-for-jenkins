pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                    . venv/bin/activate
                    echo "Building Flask application..."
                    python -m py_compile app.py
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    echo "Running tests..."
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'echo "Deploying application..."'
                sh 'docker build -t flask-jenkins-app .'
                sh 'docker run -d -p 5001:5001 flask-jenkins-app'
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
