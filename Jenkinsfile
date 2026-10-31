pipeline {
    agent {
        label 'java-maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Recuperation du code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compilation du projet'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Execution des tests'
                sh 'mvn test'
            }

            post {
                always {
                    junit(
                        testResults: 'target/surefire-reports/*.xml',
                        allowEmptyResults: false
                    )
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Generation du fichier JAR'
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        success {
            echo 'Pipeline CI termine avec succes'

            archiveArtifacts(
                artifacts: 'target/*.jar',
                fingerprint: true
            )
        }

        failure {
            echo 'Le pipeline a echoue'
        }

        always {
            echo 'Fin de l execution'
        }
    }
}