pipeline {
    agent {
        label 'w1'
    }

    environment {
        NODE_ENV = 'We AReE on PRODUCTION ENV!!!!!!!!!!!!!!!!!!!'
        GITHUB_TOKEN = credentials('jenkins-github-integration-demo.2025-03-06.private-key.pem')
    }

    stages {
        stage('Checkout') {
            steps {
                echo "$NODE_ENV"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Conditional Build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo 'Running on the main branch'
                        // Add your production build/deploy logic here
                    } else {
                        echo 'Running on a feature branch'
                        // Add logic for non-main branches here
                    }
                }
            }
        }
    }
}
