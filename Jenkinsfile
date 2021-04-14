#!/bin/groovy
pipeline {
  agent {
    kubernetes {
      defaultContainer 'jnlp'
      yaml """\
       apiVersion: v1
       kind: Pod
       metadata:
       spec:
         serviceAccountName: jenkins
         securityContext:
           runAsUser: 0
           fsGroup: 0
       
       volumes:
       - name: docker-sock
         hostPath:
           path: /var/run/docker.sock
           type: Socket
           
       containers:
       - name: docker-builder
         image: 'docker:18-git'
         command:
         - cat
         tty: true
         volumeMounts:
         - name: docker-sock
           mountPath: /run/docker.sock
         """.stripIndent()
    }
  }
  options {
    timeout(time: 3, unit: 'HOURS')
    preserveStashes(buildCount: 1)
    disableConcurrentBuilds()
  }
 
  stages {
      
    
    stage('Build') {
      steps {
        container('docker-builder') {
         
          
            sh "whoami"
            sh "pwd"
            sh "ls -al"
          
         
        }
      }
    }
  }
}
