pipeline
{
 agent any
	environment {
  	def mvnhome = tool name: 'maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn"
	def dockerrun = sh "docker run -itd -p 8081:8081 -d --name my-app esmy1990/my-app:1.0.0"
	
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
	stage("maven build")
	{
		steps
		{
			sh  "${mvncmd} clean package"
		}
	}
	stage ("docker build")
	{
		steps{
			sh "docker build -t esmy1990/my-app:1.0.0 ."
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
		
			sh "docker  push esmy1990/my-app:1.0.0"
		}
	}
	stage ("docker deployment")
	{
		steps{
			
			sshagent(['dev-server']) {
				sh "ssh -o StrictHostKeyChecking=no root@192.168.0.17 ${dockerrun}"
				}
		}
	}
	
}
}
