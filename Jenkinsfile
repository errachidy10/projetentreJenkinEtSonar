pipeline {
    agent any

    tools {
        maven 'mvn'  // Make sure you have a Maven installation configured in Jenkins with the name 'mvn'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube-Server'  // The name/URL of your SonarQube server configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/errachidy10/projetentreJenkinEtSonar.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'  // This builds the project, including running tests
            }
        }

        stage('Test') {
            steps {
                junit '**/target/surefire-reports/*.xml' // Collect JUnit test results
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_AUTH_TOKEN = credentials('sonarJenkinsToken') // The ID of your SonarQube token credential in Jenkins
            }
            steps {
                bat 'mvn sonar:sonar -Dsonar.projectKey=entre -Dsonar.host.url=$SONARQUBE_SERVER -Dsonar.login=$SONAR_AUTH_TOKEN'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}