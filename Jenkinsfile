pipeline {
    agent {
        label 'w1'  // This means it can run on any available agent (node)
    }

    environment {
        // Define environment variables here if needed
        NODE_ENV = 'production'
        GITHUB_TOKEN = credentials('demo-token')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub repository
                echo '$NODE_ENV'  // Use single quotes to print the value of the environment variable
            }
        }

        stage('Build') {
            steps {
                // Build the project (replace with your build command)
                echo 'Building the project...'
                // Example build command, replace with your own
            }
        }
    }  // Closing stages block
    
}  // Closing pipeline block
