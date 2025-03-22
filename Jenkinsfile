pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    parameters {
        choice choices: ['dev', 'prod'], name: 'SELECT_ENVIRONMENT'
    }

    stages {
        stage('build') {
            steps {
                script {
                    file = load "script.groovy"
                    file.hello()
                }
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
                        stash name: "maven_build", includes: "**/*.war"
                    }
                }
            }
        }

        stage('deploy_dev') {
            when { 
                beforeAgent true 
                expression { params.SELECT_ENVIRONMENT == 'dev' } 
            }
            agent { label 'devserver' }
            steps {
                dir("/var/www/html") {
                    unstash "maven_build"
                }
                sh """
                cd /var/www/html/
                jar -xvf webapp.war
                """
            }
        }

        stage('deploy_prod') {
            when {
                beforeAgent true
                expression { params.SELECT_ENVIRONMENT == 'prod' }
            }
            agent { label 'prodserver' }
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Deployment approved?'
                }
                dir("/var/www/html") {
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