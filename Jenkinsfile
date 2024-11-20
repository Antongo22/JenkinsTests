pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''
                set -x
                python3 -m venv venv
                chmod -R 755 venv
                . venv/bin/activate
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                set -x
                . venv/bin/activate
                echo "Python version: $(python --version)"
                echo "Pip location: $(which pip)"
                pip install -r requirements.txt
                '''
            }
        }
        stage('Check Access Rights') {
            steps {
                sh '''
                set -x
                ls -l venv/bin/pytest
                '''
            }
        }
        stage('Fix Permissions') {
            steps {
                sh '''
                set -x
                chmod +x venv/bin/pytest
                ls -l venv/bin/pytest
                '''
            }
        }
       stage('Run Tests') {
        steps {
            script {
                sh '''
                . venv/bin/activate
                export PYTHONPATH=$PYTHONPATH:/var/lib/jenkins/workspace/JTP
                python -m pytest tests/test_app.py
                '''
            }
        }
        }
        stage('Deploy') {
            steps {
                sh '''
                    echo "Deployment successful."             
                '''
            }
        }
    }
}
