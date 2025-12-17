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
	   sh 'mvn clean package'
	 }
   
   }
  
  
  
  }
 
 }