pipeline {
      agent { label 'docker_slave' }
        tools
             {
       		  git 'Default'
       		  maven 'maven'
             }
              stages{
             	  
                      stage('DockerCompose'){
                         steps{
                               script {
    sh """ssh -o StrictHostKeyChecking=no ubuntu@172.31.33.210 << EOF 
    sh 'docker-compose up -d'
    exit
    EOF"""
                                      }
                                
                              }
                      }
                      		
                     }
           }

