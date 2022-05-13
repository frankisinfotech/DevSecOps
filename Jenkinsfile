pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package -Dskiptests=true'
        archiveArtifacts 'target/*.jar'
      } 
    }
    stage ('Test') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
  }
}
         
