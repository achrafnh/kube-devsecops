pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'  //test
            }
        }   
     stage('Unit Tests - JUnit and JaCoCo') {
       steps {
         sh "mvn test"
       }
       always { 
           junit 'target/surefire-reports/*.xml'
           jacoco execPattern: 'target/jacoco.exec'
        
         }
     }
    
    }
}
