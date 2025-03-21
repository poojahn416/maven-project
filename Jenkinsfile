pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    stages {
        stage('build') {
            steps {
                echo "building theapplication"
                sh "mvn clean package"
                echo "build the application and package into jar file"
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