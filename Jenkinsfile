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
		    sh "docker build . -t smadavan/nodehello:${BUILD_NUMBER}"
            }
        }
	stage('docker push') {
	     steps {
		  script {
		      withDockerRegistry(credentialsId: 'dockerregistry') {
			      sh "docker push smadavan/nodehello:${BUILD_NUMBER}"
		       }
            }
	 }
    }
	stage('deloyment to EKS'){
        steps {
            sh "sed -i 's/BUILD_NUMBER/${BUILD_NUMBER}/g' cd/deployment.yaml"
	    sh 'kubectl apply -f cd/deployment.yaml'
        }
	}   
}
}
