pipeline {
    agent any

    environment {
        BUILD_DIR = "build"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'credentials', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        def encodedUser = URLEncoder.encode(env.GIT_USER, "UTF-8")
                        def encodedPass = URLEncoder.encode(env.GIT_PASS, "UTF-8")
                        if (isUnix()) {
                            sh "git clone https://${encodedUser}:${encodedPass}@github.com/to/repo.git"
                        } else {
                            bat "git clone https://${encodedUser}:${encodedPass}@github.com/to/repo.git"
                        }
                    }
                }
            }
        }

        stage('CMake Configure') {
            steps {
                script {
                    if (isUnix()) {
                        sh "mkdir -p ${BUILD_DIR}"
                        sh "cd ${BUILD_DIR} && cmake ../repo"
                    } else {
                        bat "if not exist ${BUILD_DIR} mkdir ${BUILD_DIR}"
                        bat "cd ${BUILD_DIR} && cmake ../repo"
                    }
                }
            }
        }

        stage('CMake Build') {
            steps {
                script {
                    if (isUnix()) {
                        // Build the project using CMake for Linux
                        sh "cd ${BUILD_DIR} && cmake --build ."
                    } else {
                        // Build the project using CMake for Windows
                        bat "cd ${BUILD_DIR} && cmake --build ."
                    }
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
