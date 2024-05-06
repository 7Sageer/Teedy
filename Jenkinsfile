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
            withDockerRegistry(credentialsId: DOCKER_CREDENTIAL_ID, url: 'https://registry.hub.docker.com') {
                docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push()
                docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push("latest")
            }
        }
    }
}

stage('Deploy') {
    steps {
        script {
            withDockerRegistry(credentialsId: DOCKER_CREDENTIAL_ID, url: 'https://registry.hub.docker.com') {
                docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").pull()
            }
            def ports = [8082, 8083, 8084]
            for (int i = 0; i < ports.size(); i++) {
                def port = ports[i]
                docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").run("-d -p ${port}:8080 --name myapp-${port}")
            }
        }
    }
}
    }
}