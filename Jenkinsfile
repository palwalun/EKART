pipeline{
 agent any
  environment {
        ACR_LOGIN_SERVER = "devopsproject1.azurecr.io"
        IMAGE_NAME = "ekart"
        TAG = "latest"
    }
   options{
    skipDefaultCheckout(true)
    }
  stages{
   stage('Checkout'){
     steps{
	  git branch: 'main', url: 'https://github.com/palwalun/EKART'
	  }
   
     }
     stage('Build'){
      steps{
	   sh 'mvn clean package -DskipTests'
	  }
   
     }
     stage('OWASP Dependency-Check') {
      steps {
         dependencyCheck additionalArguments: '--scan pom.xml', odcInstallation: 'Dependency-Check'
             dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
       }
     stage('Building Image'){
	  steps{
	    sh 'docker build -t ekart:latest .'
	  }
	 }
	 stage('Login to ACR') {
       steps {
         withCredentials([usernamePassword(
             credentialsId: 'acr-creds',
             usernameVariable: 'ACR_USER',
             passwordVariable: 'ACR_PASS'
         )]) {
             sh '''
               echo $ACR_PASS | docker login $ACR_LOGIN_SERVER \
               -u $ACR_USER --password-stdin
             '''
         }
       }
     }
      stage('Tag Image') {
        steps {
         sh '''
           docker tag ${IMAGE_NAME}:${TAG} \
           $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}
         '''
          }
        }
      stage('Push Image to ACR'){
	    steps{
	     sh 'docker push $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}'
	     }
	    }
      stage('Deploy the docker image to QA server') {
       steps {
        withCredentials([usernamePassword(
        credentialsId: 'acr-creds',
        usernameVariable: 'ACR_USER',
        passwordVariable: 'ACR_PASS'
        )]) {
         sh '''
           ssh jenkins@4.222.234.133 \
           ansible-playbook /home/jenkins/Myansible/ekart.yml \
           -e acr_username=$ACR_USER \
           -e acr_password=$ACR_PASS \
           -b
        '''
            }
           }
          }
        stage('deploying to k83'){
		     steps{
		       sh '''
		           kubectl apply -f deploymentservice.yml
		           kubectl apply -f service.yml
		         '''
             }
		       }	
          }
}