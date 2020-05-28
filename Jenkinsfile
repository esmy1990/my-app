pipeline
{
 agent any
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
		sh  "mvn clean package"
		}
	}
}
}
