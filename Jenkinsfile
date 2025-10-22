pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build and Test') {
            when {
                expression {
                    // Only run this stage if branch is 'main' or starts with 'feature'
                    return env.BRANCH_NAME == 'main' || env.BRANCH_NAME.startsWith('feature')
                }
            }
            steps {
                echo "Building and testing on branch: ${env.BRANCH_NAME}"
                bat 'dotnet build' 
                bat 'dotnet test' 
            }
            post {
                always {
                    junit 'test-results\\**\\*.xml'  // Adjust path if needed
                }
            }
        }
    }

    post {
        success {
            echo "Build and tests succeeded on branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "Build or tests failed on branch: ${env.BRANCH_NAME}"
        }
    }
}
