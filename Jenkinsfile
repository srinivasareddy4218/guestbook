pipeline {
    agent any
    //environment {
        // comment added
        // APPLICATION = 'app'
        // ENVIRONMENT = 'dev'
        // MAINTAINER_NAME = 'jenkins'
        // MAINTAINER_EMAIL = 'jenkins@email.com'
    //}
      stages {
         stage('clone repository') {
             steps {
                 // cloning repo
                 git credentialsId: 'sree-github', url: 'https://github.com/srinivasareddy4218/guestbook.git'
             }
         }
         stage('Build Docker Image'){
		 steps {
	    sh "sudo docker build -t srinivasareddy4218/php-redis:latest php-redis/"
           }
	 }		 
    stage('Build redis Docker image'){
	    steps{
	    sh "sudo docker build -t srinivasareddy4218/redis-follower:latest redis-follower/"
    }
    }
    stage('Push Docker Image'){
	    steps {
        withCredentials([string(credentialsId: 'sree-docker', variable: 'sample')]) {
          sh "sudo docker login -u srinivasareddy4218 -p ${sample}"
        }
        sh "sudo docker push srinivasareddy4218/php-redis:latest"
	     sh "sudo docker push srinivasareddy4218/redis-follower:latest"
     }
    }	    
         stage('Deploy to k8s') {
             steps {
                  sshagent(credentials : ['sshkey']) {
                      //sh "sudo scp -o StrictHostKeyChecking=no frontend-deployment.yaml cloud_user@40.117.94.170:/home/cloud_user"
                      
                         sh 'ssh -t -t root@40.117.94.170 -o StrictHostKeyChecking=no'
                      
            
                     }                                      
                        
            }
          }
       }
     }
   
 
    
