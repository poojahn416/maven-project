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
                echo "hello $NAME ${params.LASTNAME}"
            }
        }

        stage('test') {
            parallel {
                stage('testA') {
                    steps{
                        echo "this is test A"
                    }
                }

                stage('testB') {
                    steps {
                        echo "this is test B"
                    }
                }

                post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
            }
        }
    }

}