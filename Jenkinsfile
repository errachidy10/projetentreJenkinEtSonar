pipeline {
    agent any

    tools {
        // Install the Maven version needed
        maven 'mvn'
    }

    environment {
        // Define SonarQube server
        SONARQUBE_SERVER = 'SonarQube-Server'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                git url: 'https://github.com/errachidy10/simplePourTesteJenkinsEtSonarQube.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Define SonarQube authentication
                SONAR_AUTH_TOKEN = credentials('sonarJenkinsToken1')
            }
            steps {
                // Run SonarQube analysis
                bat 'mvn sonar:sonar -Dsonar.projectKey=simplePourTesteJenkinsEtSonarQube -Dsonar.host.url=$SONARQUBE_SERVER -Dsonar.login=$SONAR_AUTH_TOKEN'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                bat 'mvn test'
            }
        }
    }

    post {
        always {
            // Archive the build artifacts
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            // Publish test results
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/errachidy10/simplePourTesteJenkinsEtSonarQube.git'
            }
        }
        stage('Build') {
            steps {
                // Add your build steps here (e.g., Maven, Gradle commands)
                // Example (Maven):
                bat 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Assumes your tests generate reports in the "target/surefire-reports" directory
                junit 'target/surefire-reports/*.xml'  // Adjust the path if necessary
            }
        }
        stage('SonarQube Analysis') {
           steps {
             // Add SonarQube analysis steps here (after the build and test stages)
           }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true // Change to your output artifact(s)
        }
    }
}