pipeline {
    agent any

    environment {
        BUILD_DIR = "build"
        RELEASE_NAME="v1.0.0"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                            // withcredentials removed because of fetchcontent errorü
                            // in windows you need to login in jenkins service 
                        if (isUnix()) {
                            sh "git clone https://github.com/to/repo.git"
                        } else {
                            bat "git clone https://github.com/to/repo.git"
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
        /*
        stage('Create GitHub Release') {
                    steps {
                        script {
                            if (isUnix()) {
                                sh "tar -czvf ${RELEASE_NAME}.tar.gz -C ${BUILD_DIR} release"
                            } else {
                                bat "tar -czvf ${RELEASE_NAME}.tar.gz -C ${BUILD_DIR} release"
                            }

                            withCredentials([string(credentialsId: 'GitHubToken', variable: 'GITHUB_TOKEN')]) {
                                if (isUnix()) {
                                    sh """
                                        gh release create ${RELEASE_NAME} ${RELEASE_NAME}.tar.gz \
                                        --repo to/repo \
                                        --title "${RELEASE_NAME}" \
                                        --notes "Automated release for ${RELEASE_NAME}."
                                    """
                                } else {
                                    bat """
                                        gh release create ${RELEASE_NAME} ${RELEASE_NAME}.tar.gz ^
                                        --repo to/repo ^
                                        --title "${RELEASE_NAME}" ^
                                        --notes "Automated release for ${RELEASE_NAME}."
                                    """
                                }
                            }
                        }
                    }
        }*/

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
