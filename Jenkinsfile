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
                    venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    venv/bin/python -m py_compile app.py
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    pkill -f "python.*app.py" || true

                    nohup venv/bin/python app.py > app.log 2>&1 &

                    sleep 3

                    curl -f http://127.0.0.1:5000/health
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}
