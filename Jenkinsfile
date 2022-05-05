pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archiveArtifacts 'target/*.jar'
            }
        } 
      stage('Unit Test') {
            steps {
              sh "mvn test"
            }
        }
    }
}
