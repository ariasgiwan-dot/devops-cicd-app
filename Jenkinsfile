Jenkinsfilepipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

stage('Python Tests') {
    steps {
        sh '''
            python3 -m venv .venv
            .venv/bin/python -m pip install --upgrade pip
            .venv/bin/python -m pip install -r app/requirements.txt
            cd app
            ../.venv/bin/python -m unittest discover -s . -p "test_*.py"
        '''
    }
}
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
