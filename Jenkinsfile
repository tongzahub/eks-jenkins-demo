
pipeline{

	 environment {
    	registry = "tongzahub/eks-jenkins-demo"
    	registryCredential = 'docker-user-pass'
    	dockerImage = ''
		region = "ap-southeast-1"
		clusterName  = "arthit-devops-labs"
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

		// stage('Test image') {           
        //     app.inside {            
        //      sh 'echo "Tests passed"'        
        //     }    
        // }     

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

		stage('aws creadentials'){
			  steps {
			  
			//   withCredentials([[
   			// 		$class: 'AmazonWebServicesCredentialsBinding',
   			// 		credentialsId: "eks-credentials",
   			// 		accessKeyVariable: 'AWS_ACCESS_KEY_ID',
   			// 		secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
			// 	]]) {
   			// 		 // AWS Code
			// 	}
			  withAWS(credentials: 'eks-credentials', region: 'ap-southeast-1') {

				  sh "aws eks --region $region update-kubeconfig --name $clusterName"

				  
			  }

			  }

		}

        stage('eks deploy') {

			steps {
				sh 'echo Hello World'
				sh 'kubectl get pods'
                // sh "sed -i 's/hellonodejs:latest/hellonodejs:eks/g' deploy.yaml"
                // sh 'kubectl apply -f deploy.yaml'
                // sh 'kubectl rollout restart deployment hello-world-nodejs'
			}
		}
	}


}

