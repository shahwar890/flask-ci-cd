stage('Deploy') {
    steps {
        sh '''
        # Stop any running Flask app (optional)
        pkill -f app.py || true

        # Copy files to deployment directory
        rm -rf /home/ubuntu/flask-ci-cd-prod/*
        cp -r * /home/ubuntu/flask-ci-cd-prod/

        # Navigate and run the app in background
        cd /home/ubuntu/flask-ci-cd-prod
        python3 -m venv venv
        source venv/bin/activate
        pip install -r requirements.txt
        nohup python3 app.py > app.log 2>&1 &
        '''
    }
}
