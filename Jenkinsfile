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
        } 
    }
}
