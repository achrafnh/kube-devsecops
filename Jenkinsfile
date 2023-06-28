pipeline {
  agent any
//--------------------------
  stages {
    stage('Build Artifact') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }


    //--------------------------
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
//--------------------------
    stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
        post { 
         always { 
           pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
         }
       }
    }

//--------------------------
  stage('SonarQube - SAST') {
       steps {
         withSonarQubeEnv('SonarQube') {
           sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=myapp \
  -Dsonar.projectName='myapp' \
  -Dsonar.host.url=http://demo-test2.eastus.cloudapp.azure.com:9000 \
  -Dsonar.token=sqp_8fd0e47a1504913261b9c967bb0d703af143457d"
         }
       }
     }
//--------------------------
    stage('Docker Build and Push') {
      steps {
        withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_HUB_PASSWORD')]) {
          sh 'sudo docker login -u hrefnhaila -p $DOCKER_HUB_PASSWORD'
          sh 'printenv'
          sh 'sudo docker build -t hrefnhaila/devops-app:""$GIT_COMMIT"" .'
          sh 'sudo docker push hrefnhaila/devops-app:""$GIT_COMMIT""'
        }

      }
    }

    //--------------------------
    stage('Deployment Kubernetes  ') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
               sh "sed -i 's#replace#hrefnhaila/devops-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
               sh "kubectl apply -f k8s_deployment_service.yaml"
             }
      }

    }
    //--------------------------

  }
}
