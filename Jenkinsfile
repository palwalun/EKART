pipeline{
 agent any
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
         dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'Dependency-Check'
             dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
       }
     stage('Building Image'){
	  steps{
	    sh 'docker build -t ekart:latest .'
	  }
	 }
  
  
  }
 
 }