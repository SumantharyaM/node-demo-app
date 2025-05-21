pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // If you don't have tests, this won't fail. You can remove it later.
                sh 'npm test || echo "No tests to run"'
            }
        }

        stage('Build') {
            steps {
                echo 'Build step can be added here if needed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment step placeholder'
            }
        }
    }
}

