pipeline {
  environment {
    registry = "anandarg/devops-java"
    registryCredential = "dockerhub"
  }
  agent any
   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "M3"
   }

  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Ajay2307/spring-maven-sample.git'
      
            sh "mvn clean -Dmaven.wagon.http.ssl.insecure=true"
            sh "mvn compile -Dmaven.wagon.http.ssl.insecure=true"
            sh "mvn package -Dmaven.wagon.http.ssl.insecure=true"

      }
    }
   stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
	}
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
