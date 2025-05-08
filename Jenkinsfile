pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shahwar890/flask-ci-cd.git'
            }
        }

        stage('Setup Virtualenv') {
            steps {
                sh '''
                python3 -m venv $VENV_DIR
                source $VENV_DIR/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                source $VENV_DIR/bin/activate
                flake8 app.py
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                source $VENV_DIR/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['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']) {
                    sh '''
                    ssh user@13.201.168.89 'pkill -f app.py || true'
                    scp -r * user@13.201.168.89:/home/user/flask-ci-cd/
                    ssh user@13.201.168.89 '
                        cd /home/user/flask-ci-cd &&
                        source venv/bin/activate &&
                        nohup python3 app.py > app.log 2>&1 &
                    '
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
