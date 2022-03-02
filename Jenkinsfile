
pipeline{

	agent any
	def app   
	
	stages {

		stage('Build') {

			steps {
				// sh 'sudo docker build -t tongzahub/eks-jenkins-demo:eks .'
                app = docker.build("tongzahub/eks-jenkins-demo")    
			}
		}

		stage('Test image') {           
            app.inside {            
             sh 'echo "Tests passed"'        
            }    
        }     

		stage('Push image') {
			docker.withRegistry('https://registry.hub.docker.com', 'docker-user-pass') { 
      			 app.push("${env.BUILD_NUMBER}")            
       			 app.push("latest")        
              }    
           }
		}

        
        // stage('login') {

        //     steps {
        
		//         withCredentials([string(credentialsId: 'docker-user-pass', variable: 'PASSWORD')]) {
        //             sh 'sudo docker login -u tongzahub -p $PASSWORD'
        //         }
        //     }
        // }
		// stage('Push') {

		// 	steps {
		// 		sh 'sudo docker push tongzahub/eks-jenkins-demo:eks'
		// 	}
		// }

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


				  
			  }

			  }

		}

        stage('eks deploy') {

			steps {
				sh 'echo Hello World'
				// sh 'kubectl get -o yaml deploy/hello-world-nodejs > deploy.yaml'
                // sh "sed -i 's/hellonodejs:latest/hellonodejs:eks/g' deploy.yaml"
                // sh 'kubectl apply -f deploy.yaml'
                // sh 'kubectl rollout restart deployment hello-world-nodejs'
			}
		}
	}

	post {
		always {

            		cleanWs()
			
            		echo "done"
		}
	}

}

