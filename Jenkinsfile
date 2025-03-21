pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    parameters {
        string defaultValue: 'm', name: 'LASTNAME'
    }

    environment {
        NAME = "darshan"
    }

    stages {
        stage('build') {
            steps {
                echo "building the application"
                sh "mvn clean package"
                echo "hello &NAME &LASTNAME"
            }

            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }

}