pipeline
{
 agent any
environment {
  	def mvnhome = tool name: 'maven 3.6.3', type: 'maven'
	def mvncmd = "${mvnhome}/bin/mvn"
	withTool(toolname) {docker}
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
	
	stage("docker build")
	{
		steps{
		sh "docker.build . -t esmy1990/my-app:1.0.0"}
	}
	
}
}
