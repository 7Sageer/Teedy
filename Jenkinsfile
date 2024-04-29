pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean -DskipTests package'
            }
        }
        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
        }
        stage('Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/TEST-*.xml'
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}