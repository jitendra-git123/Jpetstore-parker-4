node {
        def majorVersion="3.0.${BUILD_NUMBER}"
	currentBuild.displayName = majorVersion

	//currentBuild.displayName = "2.0.${BUILD_NUMBER}"
	def GIT_COMMIT
  stage ('cloning the repository'){
	  
      def scm = git 'https://github.com/jitendra-git123/Jpetstore-parker-4'
	  GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
	  echo "COMMITID ${GIT_COMMIT}"
	  //echo "BBBB ${scm}"
	  //GIT_COMMIT = scm.GIT_COMMIT
	   //echo "**** ${GIT_COMMIT}"
	  
  }
	
 
  stage ('Build') {
      withMaven(jdk: 'java1.8', maven: 'Maven3.6.0') {
      sh 'mvn clean package'
	      echo "**** ${GIT_COMMIT}"
	//step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "JPetStore", requestor: "admin", id: "${newComponentVersionId}" )
	
	     
    }
  }
  
  stage ('Junit Testcase'){
  withMaven(jdk: 'java1.8', maven: 'Maven3.6.0') {
      sh 'mvn test -Dtest=Runner'	     
    }
  }

	echo("************************** Test Result Upload Started to Velocity****************************")
                        try{
                        step([$class: 'UploadJUnitTestResult',
                            properties: [
                        // Need to change the path of the test result xml result required.               
                                filePath: "target/surefire-reports/TEST-org.mybatis.jpetstore.service.OrderServiceTest.xml",
                                tenant_id: "5ade13625558f2c6688d15ce",
                                appName: "JPetStore-velocity",
                                //appExtId: "4b006cdb-0e50-43f2-ac87-a7586a65389e",
				appExtId: "e85cb6ce-96c6-4a9f-a5f8-d5deae1e2053",
				//appId: "acdfae67-616f-43e5-8872-2cfa3aa583de",    
                                name: "Executed in JUnit - 3.0.${BUILD_NUMBER}",
                                testSetName: "Junit Test Run from Jenkins"]
                           
                        ])}catch(e){
                        throw e
                        }
                       
         echo("************************** Test Result Uploaded Successful to Velocity****************************")
	
	stage('SonarQube Analysis'){
		def mvnHome = tool name : 'Maven3.6.0', type:'maven'
		//def path = tool name: 'gradle-4.7', type: 'gradle'
		
		withSonarQubeEnv('sonar-server'){
			 //"SONAR_USER_HOME=/opt/bitnami/jenkins/.sonar ${mvnHome}/bin/mvn sonar:sonar"
			//SONAR_MAVEN_GOAL -Dsonar.host.url="http://ec2-3-130-60-241.us-east-2.compute.amazonaws.com:50000
			//sh  "mvn sonar:sonar -Dsonar.projectName=JpetStore-velocity-latest1 -Dsonar.host.url=http://ec2-3-130-60-241.us-east-2.compute.amazonaws.com:50000 sonarqube"
			sh  "mvn sonar:sonar -Dsonar.projectName=JpetStore-velocity-latest7" 
			  //sh  "sonar:sonar -Dsonar.projectName=JpetStore-velocity -Dsonar.host.url=http://ec2-3-130-60-241.us-east-2.compute.amazonaws.com:50000 sonarqube"
			//sh "${path}/bin/gradle --info -Dsonar.host.url=http://localhost:9000 sonarqube"
		}
	 }
	
	
stage ("Appscan"){
     //  appscan application: '84963f4f-0cf4-4262-9afe-3bd7c0ec3942', credentials: 'Credential for ASOC', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: '84963f4f-0cf4-4262-9afe-3bd7c0ec39421562', scanner: static_analyzer(hasOptions: false, target: 'D:/Installables/Jenkins/workspace/Velocity/AltoroJ/build/libs/'), type: 'Static Analyzer'
  	build job: '/velocity/Jpetstore/asoc', wait: false, parameters: [
	build job: '/asoc', wait: false, parameters: [
	string(name: 'COMMITID', value: GIT_COMMIT),
	string(name: 'COMMITID', value: GIT_COMMIT),
	string(name: 'parentBuildNumber', value: BUILD_NUMBER)
	string(name: 'parentBuildNumber', value: 2.0.${BUILD_NUMBER})
		
	]
}
	
echo "(*******)"	
  stage('Publish Artificats to UCD'){
	  
   step([$class: 'UCDeployPublisher',
	        siteName: 'UCD_Local',
	        component: [
	            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
	            componentName: 'JPetStorevelocityComponent',
	            createComponent: [
	                $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
	                componentTemplate: '',
	                componentApplication: 'JPetStore-velocity'
	            ],
	            delivery: [
	                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
	                pushVersion: '3.0.${BUILD_NUMBER}',
	                //baseDir: '/var/jenkins_home/workspace/JPetStore/target',
			 baseDir: '/var/lib/jenkins/workspace/Velocity/Jpetstore-velocity/target/',
	                fileIncludePatterns: '*.war',
	                fileExcludePatterns: '',
	               // pushProperties: 'jenkins.server=Jenkins-app\njenkins.reviewed=false',
	                pushDescription: 'Pushed from Jenkins'
	            ]
	        ]
     ])
	  
		//sh 'env > env.txt'
	//	readFile('env.txt').split("\r?\n").each {
	//	println it
	//	}
	echo "(*******)"
	  echo "Demo1234 ${JPetStorevelocityComponent_VersionId}"
	  def newComponentVersionId = "${JPetStorevelocityComponent_VersionId}"
	  echo "git commit ${GIT_COMMIT}"
	  //step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "Altoro", requestor: "admin", id: "${newComponentVersionId}" )
 step($class: 'UploadBuild', 
       tenantId: "5ade13625558f2c6688d15ce", 
       revision: "${GIT_COMMIT}", 
       appName: "JPetStore-velocity", 
       requestor: "admin", 
       id: "${newComponentVersionId}", 
       versionName: "3.0.${BUILD_NUMBER}"
      )
     
	//echo "Demo123 ${newComponentVersionId}"
	//sleep 25
	  step([$class: 'UCDeployPublisher',
		deploy: [ createSnapshot: [deployWithSnapshot: true, 
			 snapshotName: "3.0.${BUILD_NUMBER}"],
			 deployApp: 'JPetStore-velocity', 
			 deployDesc: 'Requested from Jenkins', 
			 deployEnv: 'JPetStore-velocity_Dev', 
			 deployOnlyChanged: false, 
			 deployProc: 'Deploy-JPetStore-velocity', 
			 deployReqProps: '', 
			 deployVersions: "JPetStorevelocityComponent:3.0.${BUILD_NUMBER}"], 
		siteName: 'UCD_Local'])
 }
	//step([
          //          $class: 'UploadDeployment',
            //        debug: true,
              //      name: 'Deploy Internal Docker Compose',
                //    appName: 'JPetStore-velocity',
               //     description: "${majorVersion} to DEV",
                 //   startTime: "${currentBuild.startTimeInMillis}",
            //        endTime: "${System.currentTimeMillis()}",
              //      environmentId: "19e805ce-c40a-487e-bfdb-5b06abff4b2d",
                //    environmentName: "DEV",
                    //initiator: "${username}",
                //    result: 'success,
                  //  tenantId: "5ade13625558f2c6688d15ce",
                   // type: 'Jenkins',
                   // versionExtId: "${majorVersion}",
                   // versionName: "${majorVersion}"
                //])
	
stage ('wait for deploy') {
	sleep 25
	// echo 'Executing HCL One test ...'
	//sh '/var/jenkins_home/onetest/hcl-onetest-command.sh'
 }	

}


