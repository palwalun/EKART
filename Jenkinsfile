pipeline{
 agent any
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
            
            }
       }
  
  
  
  }
 
 }