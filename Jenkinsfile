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
  
  
  }
 
 }