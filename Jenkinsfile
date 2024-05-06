pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "qihr2022/teedy"
        DOCKER_CREDENTIAL_ID = "dockerhub-credential-id"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIAL_ID) {
                        docker.push()
                        docker.push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIAL_ID) {
                        docker.pull()
                    }

                    def ports = [8082, 8083, 8084]
                    for (int i = 0; i < ports.size(); i++) {
                        def port = ports[i]
                        docker.run("-d -p ${port}:8080 --name myapp-${port}")
                    }
                }
            }
        }
    }
}