pipeline {
    agent {
        label 'w1'
    }

    environment {
        NODE_ENV = 'production'
        GITHUB_TOKEN = credentials('jenkins-github-integration-demo.2025-03-06.private-key.pem') // GitHub token stored in Jenkins credentials
        
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
                        
                        // Create a GitHub release
                        def releaseName = "Release v${env.BUILD_NUMBER}"
                        def releaseTag = "v${env.BUILD_NUMBER}"
                        def body = "Release description for build ${env.BUILD_NUMBER}"

                        // Create a GitHub release using the GitHub API
                        def response = sh(script: """
                            curl -X POST \
                            -H "Authorization: token ${env.GITHUB_TOKEN}" \
                            -d '{"tag_name": "${releaseTag}", "name": "${releaseName}", "body": "${body}", "draft": false, "prerelease": false}' \
                            ${env.GITHUB_API_URL}/repos/${env.GITHUB_REPO}/releases
                        """, returnStdout: true).trim()

                        echo "Release created: ${response}"
                    } else {
                        echo 'Running on a feature branch'
                    }
                }
            }
        }
    }
}
