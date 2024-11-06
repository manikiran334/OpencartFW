pipeline {
    agent any

    environment {
        ENV_FILE = "config/env/${params.ENV}.yml"
    }

    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select environment')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the OpenCart project...'
                sh './scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh './scripts/run_tests.sh'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying the OpenCart application...'
                sh './scripts/deploy.sh'
            }
        }
    }

    post {
        success {
            echo 'Build and deployment succeeded!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}
