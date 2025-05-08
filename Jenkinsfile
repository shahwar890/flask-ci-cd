pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        DEPLOY_DIR = '/home/ubuntu/flask-ci-cd-prod'
        APP_ENTRY = 'app.py'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shahwar890/flask-ci-cd.git'
            }
        }

        stage('Setup Virtual Environment') {
    steps {
        sh '''
        python3 -m venv venv
        . venv/bin/activate
        pip install --upgrade pip
        pip install -r requirements.txt
        '''
    }
}

        stage('Lint') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                flake8 $APP_ENTRY
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Stopping any existing Flask process..."
                pkill -f $APP_ENTRY || true

                echo "Deploying to $DEPLOY_DIR..."
                rm -rf $DEPLOY_DIR/*
                cp -r * $DEPLOY_DIR/

                echo "Starting Flask app..."
                cd $DEPLOY_DIR
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                nohup python3 $APP_ENTRY > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Something went wrong during the pipeline.'
        }
    }
}
