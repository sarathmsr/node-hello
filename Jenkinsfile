pipeline{
    agent any

    tools {
         nodejs 'node'
    }

    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitcreds', url: 'git@github.com:sarathmsr/node-hello.git']]])
            }
        }
        stage('build'){
            steps{
               sh 'npm install'
            }
        }
	stage('dockerbuild'){
	    steps{
		    sh "docker build . -t nodehello:${BUILD_NUMBER}"
            }
        }
	stage('docker push') {
	     steps {
		  script {
		       docker.withRegistry() {
			       docker.image().push()
		       }
            }
	 }
    }
}
