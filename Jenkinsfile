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
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
           }
         }
      }
       stage('Build DOcker Image') {
            steps {
              sh 'printenv'
              sh 'docker build -t frankisinfotech/springboot:v1 ""$GIT_COMMIT"" .'
            }
        } 
  }
}
