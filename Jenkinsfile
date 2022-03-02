
pipeline{

	 environment {
    	registry = "tongzahub/eks-jenkins-demo"
    	registryCredential = 'docker-user-pass'
    	dockerImage = ''
		region = "ap-southeast-1"
		clusterName  = "arthit-devops-labs"
		eksProfile = "eks-credentials"
  	}

	agent any
	
	stages {


		stage('Build') {
			steps {
			script {
         		 dockerImage = docker.build registry + ":$BUILD_NUMBER"
       		 }   
			}
		}
  

		stage('Push image') {
			steps {
				 script {
            		docker.withRegistry( '', registryCredential ) {
           			dockerImage.push()
         		 }
			}
		}

		}
        
		stage('Remove Unused docker image') {
     		 steps{
        		sh "docker rmi $registry:$BUILD_NUMBER"
     	 	}
   		}

		stage('deploy EKS'){
			  steps {
			  withAWS(credentials: "$eksProfile" , region: 'ap-southeast-1') {
				  sh "aws iam list-account-aliases"
				  sh "aws eks --region $region update-kubeconfig --name $clusterName"
				  sh 'kubectl get pods'
				  sh 'kubectl get nodes'
				  sh "kubectl apply -f eks-example-deployment.yaml"
				  
			  }

			  }

		}
	}


}

