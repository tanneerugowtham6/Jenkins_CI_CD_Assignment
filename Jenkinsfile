pipeline {
    agent any

    stages {
        stage('Build') {
            echo 'Installing the dependencies...'
            sh 'cd /Python_Web_App'
            sh 'pip install -r requirements.txt'
        }
        stage('Test') {
            echo 'Testing the application...'
            sh 'cd /Python_Web_App'
            sh 'pytest || true'
        }
        stage('Deploy') {
            echo 'Deploying the application...'
            sh 'cd /Python_Web_App'
            sh 'python3 app.py'
        }
    }
}
