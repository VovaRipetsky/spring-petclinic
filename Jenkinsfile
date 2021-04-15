pipeline {
    agent none    
    
    
    options {
    timeout(time: 3, unit: 'HOURS')
    preserveStashes(buildCount: 1)
    disableConcurrentBuilds()
  }
  environment {
    AWS_REGION    = "us-east-2"
    ECR_URL       =  "https://676833452478.dkr.ecr.us-east-2.amazonaws.com/myapp"
    IMAGE_TAG     = "676833452478.dkr.ecr.us-east-2.amazonaws.com/myapp:java_v_${env.BUILD_ID}"  
  // VERSION       = getVersion() 
    
  }
    
    
    stages {
         stage('Build JAR file by Maven') {            
           agent {
               docker {
                   image 'maven:3.6.0-jdk-8-alpine' 
                   args '-v /root/.m2:/root/.m2'
               }  
           }            
            steps {
              //  sh 'mvn package'
              //  stash(name: "artifact", includes: '**/target/*.jar')
             //   archiveArtifacts artifacts: '**/target/*.jar'
                  }
            }
        
        stage('Build Docker Image & Push to ECR') {
            agent {
               docker {
                   image 'docker:latest' 
                   args '-v /var/run/docker.sock:/var/run/docker.sock'
               }    
            }
                         steps{
                              script{
                                 //    currentBuild.displayName = getDisplayName(VERSION)
                                    
                      //               unstash 'artifact'
                                  //   docker.withRegistry('${ECR_URL}', 'ecr_key') 
                                  withDockerRegistry(credentialsId: 'ecr:us-east-2:ecr_key', url: "${env.ECR_URL}") 
                                     {
                                     // need sudo chmod 666 /var/run/docker.sock from host

                                     def customImage = docker.build("${env.IMAGE_TAG}")
                                     /* Push the container to the custom Registry */
                                     customImage.push()
                                     }
                                    }
                              }
                      
                      }
        
          stage('Run Container with built image') {            
              agent any
               steps {
                   sh "docker run -d -p 80:80 ${env.IMAGE_TAG}"
                     }
                                                  }

          }
    
    
  
}
//def getVersion() {
//  shortCommit = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
//  commitCount = sh(returnStdout: true, script: 'git rev-list --count HEAD').trim()
//  return "${commitCount}-${shortCommit}"
//}
