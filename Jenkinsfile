// Obtaining an Artifactory server instance defined in Jenkins:
			
// def server = Artifactory.server 'Artifactory Version 4.15.0'

		 //If artifactory is not defined in Jenkins, then create on:
		// def server = Artifactory.newServer url: 'Artifactory url', username: 'username', password: 'password'

//Create Artifactory Maven Build instance
def rtMaven = Artifactory.newMavenBuild()

def buildInfo

pipeline {
    agent any

	stages {
        stage('Clone sources'){
            steps {
                git url: 'https://github.com/Nagendra-ch/jenkins2-course-spring-boot.git'
            }
        }

     	stage('SonarQube analysis') {
	    //  steps {
		//Prepare SonarQube scanner enviornment
		// withSonarQubeEnv('credentialsId: 'b6ba01fd-60a3-438e-8920-4db0b9119fa8', installationName: 'sonarqube') 
		withSonarQubeEnv(credentialsId: 'b6ba01fd-60a3-438e-8920-4db0b9119fa8', installationName: 'sonarqube')
		sh 'mvn sonar:sonar \
			-Dsonar.projectKey=jenkins2-course-spring-boot \
			-Dsonar.host.url=http://192.168.56.109:9000 \
			-Dsonar.login=11464c0f4cc861c21b3db9dea8a894d48e6e2b72'

//	stage('Quality Gate') {
//		steps {
//			timeout(time: 1, unit: 'HOURS') {
			//Parameter indicates wether to set pipeline to UNSTABLE if Quality Gate fails
		        // true = set pipeline to UNSTABLE, false = don't
			// Requires SonarQube Scanner for Jenkins 2.7+
//			waitForQualityGate abortPipeline: false
//		       }
//		 }
		}

	// stage('Artifactory configuration') {
		
	//    steps {
	// 	script {
	// 		rtMaven.tool = 'Maven-3.6.3' //Maven tool name specified in Jenkins configuration
		
	// 		rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server //Defining where the build artifacts should be deployed to
			
	// 		rtMaven.resolver releaseRepo:'libs-release', snapshotRepo: 'libs-snapshot', server: server //Defining where Maven Build should download its dependencies from
			
	// 		rtMaven.deployer.artifactDeploymentPatterns.addExclude("pom.xml") //Exclude artifacts from being deployed
			
	// 		//rtMaven.deployer.deployArtifacts =false // Disable artifacts deployment during Maven run
		    
	// 		buildInfo = Artifactory.newBuildInfo() //Publishing build-Info to artifactory
			
	// 		buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true

	// 		buildInfo.env.capture = true
	// 		}
	//     }
	// }

	stage('Execute Maven') {
		steps {
		   script {
		
		rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
			}
		}
		
	}

	stage('Publish build info') {
		steps {
		  script {

		server.publishBuildInfo buildInfo
		}
		}
	}
}
}