pipeline {
  agent any
  environment {
        AWS_ACCOUNT_ID="985729960198"
        AWS_DEFAULT_REGION="eu-west-2" 
        IMAGE_REPO_NAME="frankdemo"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }

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
       stage('Build Docker Image') {
            steps {
                sh 'printenv'
                sh 'docker build -t frankisinfotech/springboot:""$GIT_COMMIT"" .'
              }
            } 
      // stage('Push to DockerHub') {
      //      steps {
      //        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
      //          sh 'docker push frankisinfotech/springboot:""$GIT_COMMIT""'
      //        }
      //      }
      //  }
        stage('Logging to AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login -u frank --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
                 
            }
        }
         stage('Build image for ECR') {
             steps{
                 script {
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                        }
                  }
            }
        stage('Push to AWS ECR') {
            steps {
                script {
                  sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                  sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"  
              }
            }
          }
        
  }
}
