pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    stages {
        stage('build') {
            steps {
                echo "building the application"
                sh "mvn clean package"
                echo "application build successfully"
            }

            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                    echo "artifacts are stored successfully"
                }
            }
        }
    }
}