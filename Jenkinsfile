node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'mvn';
    withSonarQubeEnv() {
      bat "\"${mvn}\\bin\\mvn\" clean verify sonar:sonar -Dsonar.projectKey=test -Dsonar.projectName='test'"
    }
  }
}
