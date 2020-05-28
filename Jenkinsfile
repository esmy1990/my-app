pipeline
{
 agent any
environment {
  	def mvnhome = tool name: 'Maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn
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
}
}
