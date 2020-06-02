pipeline
{
 agent any
	environment {
  	def mvnhome = tool name: 'maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn"
	}
 stages
{
	stage("checkout") 
	{
		steps
		{
		git 'https://github.com/esmy1990/my-app'
		}
	}
	
	stage("tests"){
	parallel {
	
       stage("maven build")
	{
		steps
		{
			sh  "${mvncmd} clean package"
		}
	}
    
        stage ("sonar")
	{
		steps{
		withSonarQubeEnv("sonar")
			{
				sh "${mvncmd} clean package sonar:sonar"
			}
		}
	}
    }
   	   }

	stage ("docker build")
	{
		steps{
			sh "docker build -t esmy1990/my-app:2.0.0 ."
		}
	}
	stage ("docker push")
	{
		
		steps
		{
			withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerpwd')]) 
			{
		
			sh "docker login -u esmy1990 -p ${dockerpwd}"
			}
		
			sh "docker  push esmy1990/my-app:2.0.0"
		}
	}
		
}
}
