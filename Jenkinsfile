def rtMaven = Artifactory.newMavenBuild()

def buildInfo
node {
stage ('Init'){
  checkout scm
  sh 'echo $BRANCH_NAME'
}
  stage('SClone sources - CM') {
    git 'https://github.com/sureshaws2143/jenkins2-course-spring-boot.git'
  }
  stage('SonarQube analysis') {
        if (env.BRANCH_NAME == 'master') {
        stage 'Only on master'
        println 'This happens only on master'
        } else {
        stage 'Other branches'
        println "Current branch ${env.BRANCH_NAME}"
        }
          withSonarQubeEnv(credentialsId: 'b6ba01fd-60a3-438e-8920-4db0b9119fa8', installationName: 'sonarqube') { // You can override the credential to be used
          //sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
        sh 'mvn sonar:sonar \
			-Dsonar.projectKey=jenkins2-course-spring-boot \
			-Dsonar.host.url=http://192.168.56.109:9000 \
			-Dsonar.login=11464c0f4cc861c21b3db9dea8a894d48e6e2b72'
          }
        }

		stage('Maven Build'){		
		rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
		}
		

		stage('Publish build info'){
		server.publishBuildInfo buildInfo
		}

}
// sh 'mvn sonar:sonar \
//   -Dsonar.projectKey=sonarscanner-maven-basic \
//   -Dsonar.host.url=http://10.1.3.29:9000/sonarqube \
//   -Dsonar.login=5d7694152e415c79121e161e461066048016117d'

// sh 'mvn sonar:sonar \
//   -Dsonar.projectKey=sonarscanner-maven-basic \
//   -Dsonar.host.url=http://10.1.3.29:9000/sonarqube \
//   -Dsonar.login=1141d5896f177861ef5b06bd7fc
