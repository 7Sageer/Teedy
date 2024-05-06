pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("qihr2022/teedy:24")
                }
            }
        }
        
        stage('Push') {
            steps {
                // script {
                //     withDockerRegistry(credentialsId: 'dockerhub-credential-id', url: 'https://registry.hub.docker.com') {
                //         docker.image("qihr2022/teedy:24").push()
                //         docker.image("qihr2022/teedy:24").push("latest")
                //     }
                // }
                script {
                    sleep 5
                }
                echo "Push to Docker Hub"
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    def ports = [8082, 8083, 8084]
                    withDockerRegistry(credentialsId: 'dockerhub-credential-id', url: 'https://registry.hub.docker.com') {
                        for (int i = 0; i < ports.size(); i++) {
                            def port = ports[i]
                            sh "docker run -d --name teedy-${port} -p ${port}:8080 qihr2022/teedy:24"
                            echo "Application is running on port ${port}"
                        }
                    }
                }
            }
        }
        
        stage('Clean') {
            steps {
                script {
                    sh "docker rmi qihr2022/teedy:24"
                }
            }
        }
    }
    
    post {
        failure {
            echo 'Pipeline failed.'
        }
        unstable {
            echo 'Pipeline is unstable.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
    }
}