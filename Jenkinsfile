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
            }

            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }

}