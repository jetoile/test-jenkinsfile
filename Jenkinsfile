#!/usr/bin/env/groovy


stage 'Build'
	checkout scm
	
	def mvnHome = tool name: 'mvn-3.3.9', type: 'maven'
	dev javaHome = tool name: 'jdk1.8u112', type: 'jdk'
	env.PATH = "${javaHome}/bin:${mvnHome}/bin:${env.PATH}"

	sh 'mvn clean deploy'

	if (env.BRANCH_NAME == "master") {
		stage 'Approve'
			def originalV = version() 
			def major = originalV[0]
			def minor = originalV[1].toInteger() + 1
			def minor2 = originalV[1]
			def v = "${major}.${minor}"
			if (v) {
				echo "Building version ${v}"
			}
			
			def userInput = true
			def didTimeout = false

			try {
				timeout(time: 10, unit : 'MINUTES') {
					userInput = input(
							id: 'userInput',
							message: 'Do you want ot release?',
							parameters: [
									[$class: 'TextParameterDefinition', defaultValue: "$major.$minor-SNAPSHOT", description: 'next dev', name: 'devVersion'],
									[$class: 'TextParameterDefinition', defaultValue: "$major.$minor2", description: 'rekease versuib', name: 'releaseVersion'],
									[$class: 'TextParameterDefinition', description: 'username', name: 'username'],
									[$class: 'TextParameterDefinition', description: 'password', name: 'password']
							]
					)
				}
			} catch(err) {
				didTimeout = true
				userInput = false
				echo 'Aborded by [$user] or timeout'
			}

		stage 'Deploy'
			if (didTimeout) {
				echo "no input was received before timeout"
			} else if (userInput == true) {
				echo "this is successful"
			} else {
				def releaseVersion = userInput['releaseVersion']
				def devVersion = userInput['devVersion']
				def username = userInput['username']
				def password = userInput['password']

				def pomArtifact = "toto"
				sh "mvn --batch-mode release:prepare release:perform -DreleaseVersion=${releaseVersion} -DdevelopmentVersion=${devVersion} -Dusername=${username} -Dpassword=${password}"

			}
	}

def version() {
	def matcher = readFile('pom.xml') =~ '<version>(.+)-.*</version>'
	matcher ? matcher[0][1].tokenize(".") : null
}
