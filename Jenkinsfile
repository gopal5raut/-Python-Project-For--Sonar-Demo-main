pipeline {
    agent any
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gopal5raut/-Python-Project-For--Sonar-Demo-main.git'
            }
        }
        
stage('Setup Virtual Environment') {
    steps {
        sh '''
        rm -rf venv  # Remove any existing virtual environments
        python3 -m venv venv  # Create a new virtual environment
        chmod -R 755 venv     # we are setting up the permission for jenkins user as it runs the pipeline
        bash -c "             # so that it should be executed by bash
        source venv/bin/activate # activate a Python virtual environment
        pip install --upgrade pip #Upgrades pip to the latest version from the Python Package Index (PyPI).
        pip install -r requirements.txt  #Pip installs packages form requirements.txt into the current Python environment of the system it's running on.
        "
        '''
    }
}
   
        stage('Test') {
            steps {
                sh '''
                bash -c "
                source venv/bin/activate
                pytest --cov=app --cov-report=xml
				pytest --cov=app --cov-report=term-missing --disable-warnings
                "
                '''
            }
        }
        
         stage('Sonar') {
            steps {
                withSonarQubeEnv('sonar-server') {
                          sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Python-project \
                          -Dsonar.projectName=Python-project \
                          -Dsonar.exclusions=venv/** \
                          -Dsonar.sources=. \
                          -Dsonar.python.coverage.reportPaths=coverage.xml'''
                }
            }
        }
    }
}
