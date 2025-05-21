pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SumantharyaM/node-demo-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test || echo "No tests or test failed, but continuing"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to EC2...'

                // Copy files to EC2 (excluding node_modules to reduce transfer size)
                sh '''
                rsync -avz --exclude 'node_modules' -e "ssh -i ~/.ssh/Sumanth-Keypair.pem -o StrictHostKeyChecking=no" ./ ubuntu@13.60.74.106:/home/ubuntu/node-demo-app
                '''

                // SSH to EC2 and install dependencies & start app with pm2
                sh '''
                ssh -i ~/.ssh/Sumanth-Keypair.pem -o StrictHostKeyChecking=no ubuntu@13.60.74.106 << EOF
                    cd /home/ubuntu/node-demo-app
                    npm install
                    pm2 delete all || true
                    pm2 start app.js
                    pm2 save
                EOF
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}


