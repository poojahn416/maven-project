pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    parameters {
        choice choices: ['dev', 'prod'], name: 'SELECT_ENVIRONMENT'
    }

    environment {
        NAME = "darshan"
    }

    stages {
        stage('build') {
            steps {
                echo "building the application"
                sh "mvn clean package -DskipTests=true"
            }
        }

        stage('test') {
            parallel {
                stage('testA') {
                    agent { label 'devserver'}
                    steps{
                        echo "this is test A"
                        sh "mvn test"
                    }
                }

                stage('testB') {
                    agent { label 'devserver'}
                    steps {
                        echo "this is test B"
                        sh "mvn test"
                    }
                }
            }
            post {
                success {
                    dir("webapp/target/") {
                        stash name: "maven_build", includes: "*.war"
                    }
                }
            }
        }

        stage('deploy_dev') {
            when { expression { params.SELECT_ENVIRONMENT == 'dev' }
            beforeAgent true }
            agent { label 'devserver' }
            steps {
                dir("/var/www") {
                    unstash "maven_build"
                }
                sh """
                cd /var/www/html/
                jar -xvf webapp.war
                """
            }
        }
    }

}