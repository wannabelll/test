pipeline {
    agent {
        label 'w1'
    }

    environment {
        NODE_ENV = 'TESTTTTTTTTTTTTT'
        GITHUB_TOKEN = credentials('jenkins-github-integration-demo.2025-03-06.private-key.pem') // GitHub token stored in Jenkins credentials
        GITHUB_API_URL = 'https://api.github.com' // GitHub API base URL
    }

    stages {
        stage('Checkout') {
            steps {
                // This step ensures the repository is checked out before continuing
                checkout scm
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

                        // Dynamically extract the repo name and owner from the Git URL
                        def gitUrl = sh(script: "git config --get remote.origin.url", returnStdout: true).trim()
                        def repoInfo = gitUrl.replaceAll('git@github.com:', '').replaceAll('.git$', '').split('/')
                        def repoOwner = repoInfo[0]
                        def repoName = repoInfo[1]

                        // Create the release information
                        def releaseName = "Release v${env.BUILD_NUMBER}"
                        def releaseTag = "v${env.BUILD_NUMBER}"
                        def body = "Release description for build ${env.BUILD_NUMBER}"

                        // Create a GitHub release using the GitHub API
                        def response = sh(script: """
                            curl -X POST \
                            -H "Authorization: token ${env.GITHUB_TOKEN}" \
                            -d '{"tag_name": "${releaseTag}", "name": "${releaseName}", "body": "${body}", "draft": false, "prerelease": false}' \
                            ${env.GITHUB_API_URL}/repos/${repoOwner}/${repoName}/releases
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

