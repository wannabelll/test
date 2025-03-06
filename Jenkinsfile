pipeline {
    agent { label 'w1' }

    environment {
        GITHUB_API_URL = 'https://api.github.com'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Create Release') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'github-app-private-key', variable: 'PRIVATE_KEY_FILE')]) {
                        // Use the private key stored in Jenkins Credentials
                        def jwt = sh(script: """
                            # Create JWT for GitHub App authentication
                            curl -X POST -H "Content-Type: application/json" \
                            -d '{"iat": $(date +%s), "exp": $(($(date +%s) + 600)), "iss": "your-app-id"}' \
                            -o jwt.json \
                            https://api.github.com/app/installations/${GITHUB_INSTALLATION_ID}/access_tokens
                        """, returnStdout: true).trim()

                        def token = readJSON(text: jwt).token

                        // Now use the token to create a release
                        def response = sh(script: """
                            curl -X POST \
                            -H "Authorization: token ${token}" \
                            -d '{"tag_name": "v1.0", "name": "Release v1.0", "body": "Release description", "draft": false, "prerelease": false}' \
                            ${GITHUB_API_URL}/repos/${repoOwner}/${repoName}/releases
                        """, returnStdout: true).trim()

                        echo "Release created: ${response}"
                    }
                }
            }
        }
    }
}
