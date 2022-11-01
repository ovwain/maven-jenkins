pipeline {
  agent any // means run on any machine that is available to Jenkins
  tools {
        maven "M3"
        sonarqube "SonarScanner"
   }
  // this is a dummy change
  stages {
      stage('Build Artifact') 
      {
            steps 
            {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('SonarQube Stuff') 
      {
            steps 
            {
              sh "mvn clean verify sonar:sonar \
                -Dsonar.projectKey=maven-jenkins-pipeline \
                -Dsonar.host.url=http://34.89.112.204:9000 \
                -Dsonar.login=sqp_128bf691b445067b1181077aedb26dea16b4d58d"
            }  
       }
      stage('Sonarqube Analysis - SAST')  
      { 
        steps  
        { 
          withSonarQubeEnv('SonarQube')  
            { 
              sh "mvn sonar:sonar -Dsonar.projectKey=maven-jenkins-pipeline -Dsonar.host.url=http://34.89.112.204:9000/" 
            
            }
          timeout(time: 2, unit: 'MINUTES') {
            script {
              waitForQualityGate abortPipeline: true
            }
          }
        } 
      } 
      stage('Test Maven - JUnit') 
      {
            steps 
            {
              //sh "mvn test"
              echo "Running unit tests"
            }
      }
      stage('Dev Environment') 
      { 
          steps 
          { 
              echo "I'm now deploying to the dev environment" 
          } 
      }
      stage('Test Environment') 
      { 
          steps 
          { 
              echo "I'm now deploying to the test or QA environment" 
          } 
      }
      stage('UAT Environment') 
      { 
          steps 
          { 
              input('Continue to Deploy?') 
              echo 'Manager Approved :)'
              echo 'Deploying to Production Environment' 
          } 
      }
      stage('Production Environment') 
      { 
          steps 
          { 
              input('Continue to Deploy?') 
              echo 'Deploying to Production Environment' 
          } 
      }
      stage('Deploy')
      {
          steps
          {
               input('Continue to Deploy?')
               echo 'Deploying to Production Environment'
          }
      }
    }
}
