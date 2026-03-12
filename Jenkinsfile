pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                sh '''
                    . venv/bin/activate
                    # Run checks
                    # E402: Module level import not at top of file (needed for sys.path hack in tests)
                    flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
                    flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics --ignore=E402
                    # Run tests with coverage
                    pytest --verbose --cov=app tests/
                '''
            }
        }

        stage('Deploy Simulation') {
            steps {
                echo 'Simulating deployment...'
                sh 'sleep 2'
                echo 'Deployment successful!'
            }
        }
    }
}
