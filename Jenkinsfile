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
	stage ("sonar")
	{
		steps{
		withSonarQubeEnv("sonar")
			{
				sh "${mvncmd} clean package sonar:sonar"
			}
		}
	}
	
	stage('checkmarx') {
  environment {
    CX_CREDS  = credentials('jenkins-checkmarx-cxsast-creds')
  }
  steps{
    echo "Executing Checkmarx Jenkins Plugin to request a Scan..."
    step([$class: 'CxScanBuilder', comment: '', excludeFolders: '', excludeOpenSourceFolders: '', exclusionsSetting: 'job',
      filterPattern: '''!**/_cvs/**/*, !**/.svn/**/*,   !**/.hg/**/*,   !**/.git/**/*,  !**/.bzr/**/*, !**/bin/**/*,
        !**/obj/**/*,  !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr,     !**/*.iws,
        !**/*.bak,     !**/*.tmp,       !**/*.aac,      !**/*.aif,      !**/*.iff,     !**/*.m3u, !**/*.mid, !**/*.mp3,
        !**/*.mpa,     !**/*.ra,        !**/*.wav,      !**/*.wma,      !**/*.3g2,     !**/*.3gp, !**/*.asf, !**/*.asx,
        !**/*.avi,     !**/*.flv,       !**/*.mov,      !**/*.mp4,      !**/*.mpg,     !**/*.rm,  !**/*.swf, !**/*.vob,
        !**/*.wmv,     !**/*.bmp,       !**/*.gif,      !**/*.jpg,      !**/*.png,     !**/*.psd, !**/*.tif, !**/*.swf,
        !**/*.jar,     !**/*.zip,       !**/*.rar,      !**/*.exe,      !**/*.dll,     !**/*.pdb, !**/*.7z,  !**/*.gz,
        !**/*.tar.gz,  !**/*.tar,       !**/*.gz,       !**/*.ahtm,     !**/*.ahtml,   !**/*.fhtml, !**/*.hdm,
        !**/*.hdml,    !**/*.hsql,      !**/*.ht,       !**/*.hta,      !**/*.htc,     !**/*.htd, !**/*.war, !**/*.ear,
        !**/*.htmls,   !**/*.ihtml,     !**/*.mht,      !**/*.mhtm,     !**/*.mhtml,   !**/*.ssi, !**/*.stm,
        !**/*.stml,    !**/*.ttml,      !**/*.txn,      !**/*.xhtm,   !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*''',
      fullScanCycle: 10,
      fullScansScheduled: true,
      generatePdfReport: true,
      groupId: '00000000-1111-1111-b111-989c9070eb11',
      includeOpenSourceFolders: '',
      osaEnabled: false,
      username: "${CX_CREDS_USR}",
      password: "${CX_CREDS_PSW}",
      preset: '36',
      projectName: "${params.CX_PROJECT_NAME}",
      serverUrl: "${params.CX_SERVER_URL}",
      sourceEncoding: '1',
      waitForResultsEnabled: true,
      vulnerabilityThresholdEnabled: true,
      vulnerabilityThresholdResult: 'UNSTABLE',
      highThreshold: 1,
      lowThreshold: 1,
      mediumThreshold: 1,
      generatePdfReport: false])
  }
	
}
}
