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
                    archieveArtifacts artifacts:'**/target/*.jar'
                    sh "artifacts are stored successfully"
                }
            }
        }
    }
}