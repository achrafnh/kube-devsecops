pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        }
        stage('UNIT test & jacoco ') {
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



    stage('Docker Build and Push') {
      steps {
          withDockerRegistry(credentialsId: 'dockerhubnhaila', url:'') {
              sh 'printenv'
              sh 'sudo docker build -t hrefnhaila/numeric-app:""$GIT_COMMIT"" .'
              sh 'docker push hrefnhaila/numeric-app:""$GIT_COMMIT""'
          }
        
      }
    }



    
    }
}
