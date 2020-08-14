node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALSS', url: 'https://github.com/manikarnam/guestbook.git', branch: 'master'
    }
	
	stage('Whoami'){
	  sh "whoami"
		
	}
    
    stage('Build Docker Image'){
	    sh "sudo docker build -t maniengg/php-redis:${BUILD_ID} php-redis/"
    }
    stage('Build redis Docker image'){
	    sh "sudo docker build -t maniengg/redis-follower:${BUILD_ID} redis-follower/"
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "sudo docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        }
        sh "sudo docker push maniengg/php-redis:${BUILD_ID}"
	sh "sudo docker push maniengg/redis-follower:${BUILD_ID}"
     }
     
    /** stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'mykubeconfig',
         enableConfigSubstitution: true
        )
     }**/
	 
     stage("Deploy To Kuberates Cluster"){
        withCredentials([file(credentialsId: 'demo-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
         sh "gcloud config set project mssdevops-284216"
         sh "gcloud config set compute/zone us-central1-c"
         sh "gcloud config set compute/region us-central1"
         sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
         //sh "sed -i -e 's,image_to_be_deployed,'maniengg/spring-boot-mongo:${BUILD_ID}',g' springBootMongo.yml"
         sh "kubectl apply -f frontend-deployment.yaml"      
         sh "kubectl apply -f frontend-service.yaml"
	 sh "kubectl apply -f redis-follower-deployment.yaml"
         sh "kubectl apply -f redis-follower-service.yaml"
	 sh "kubectl apply -f redis-leader-deployment.yaml"
	 sh "kubectl apply -f redis-leader-service.yaml"
        }
      }
}
