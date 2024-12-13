pipeline {
    agent any

    environment {
        BUILD_DIR = "build"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Index of jenkins credentials
                    withCredentials([usernamePassword(credentialsId: 'credentials', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        username and secret
                        def encodedUser = URLEncoder.encode(env.GIT_USER, "UTF-8")
                        def encodedPass = URLEncoder.encode(env.GIT_PASS, "UTF-8")
                        bat "git clone https://${encodedUser}:${encodedPass}@github.com/to/repo.git"
                    }
                }
            }
        }

        stage('CMake Configure') {
            steps {
                script {
                    // Create build directory if it does not exist
                    bat "if not exist ${BUILD_DIR} mkdir ${BUILD_DIR}"
                    // Navigate to build directory and run cmake
                    bat "cd ${BUILD_DIR} && cmake ../repo"
                }
            }
        }

        stage('CMake Build') {
            steps {
                script {
                    // Build the project using CMake
                    bat "cd ${BUILD_DIR} && cmake --build ."
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace"
            cleanWs()
        }
        success {
            echo "Pipeline completed successfully."
        }
        failure {
            echo "Pipeline failed. Check the logs for errors."
        }
    }
}
