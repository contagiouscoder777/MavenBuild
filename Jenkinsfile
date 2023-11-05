[ERROR] Failed to execute goal org.apache.maven.plugins:maven-war-plugin:3.0.0:war (default-war) on project demo: Execution default-war of goal org.apache.maven.plugins:maven-war-plugin:3.0.0:war failed: Unable to load the mojo 'war' in the plugin 'org.apache.maven.plugins:maven-war-plugin:3.0.0' due to an API incompatibility:



node('') {
	stage ('checkout code'){
		checkout scm
	}

	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}
node('Slave'){

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		//sh 'mvn sonar:sonar -Dsonar.host.url=http://35.153.67.119:9000 -Dsonar.login=77467cfd2653653ad3b35463fbfdb09285f08be5'
	def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

	stage('Code Checkout'){
		checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHubCreds', url: 'https://github.com/anujdevopslearn/MavenBuild']])
	}
	stage('Build Automation'){
		sh """
			ls -lart
			mvn clean install
			ls -lart target
	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
		"""
	}

	stage ('Deployment'){
		ansiblePlaybook become: true, colorized: true, disableHostKeyChecking: true, playbook: 'deploy.yml'

	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "build-alerts@example.com"
		    )
	stage('Code Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:7080/')], contextPath: 'exp5mvnPipeline', onFailure: false, war: 'target/*.war'
	}
}
