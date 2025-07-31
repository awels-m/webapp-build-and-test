pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out your repository...'
                checkout scm
            }
        }
        
        stage('Build Application') {
            steps {
                echo 'Verifying index.html exists...'
                sh '''
                if [ -f "index.html" ]; then
                    echo "index.html found, build successful."
                else
                    echo "index.html missing, build failed."
                    exit 1
                fi
                '''
            }
        }
        
        stage('Unit Tests') {
            steps {
                echo 'Running basic unit test...'
                sh '''
                grep -q "<html>" index.html && echo "Test Passed: HTML tag found!" || (echo "Test Failed: HTML tag missing!"; exit 1)
                '''
            }
        }
        
        stage('Deploy Web Application') {
            steps {
                echo 'Deploying your web application...'
                sh '''
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart nginx
                '''
            }
        }
    }
}

