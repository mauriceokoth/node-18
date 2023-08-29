pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Clone your source code repository
                    checkout scm

                    // Build the Docker image
                    docker.build("node:18.17.1-alpine3.18", "--file Dockerfile .")
                }
            }
        }

        stage('Tag and Push') {
            steps {
                script {
                    // Tag the Docker image
                    docker.image("node:18.17.1-alpine3.18").tag("${env.BUILD_NUMBER}")

                    // Log in to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        // Push the Docker image to Docker Hub
                        docker.image("node:18.17.1-alpine3.18").push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            script {
            def dockerImage = docker.image("node:18.17.1-alpine3.18")
            dockerImage.pull()
            dockerImage.remove()
            }
        }
    }
}
