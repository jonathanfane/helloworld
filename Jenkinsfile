node {
    def app
	def deployment = "rolling" 

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("jonathanfane/helloworld")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
	
    stage('Deploy ') {
    
	 sleep 5
	
     if (deployment == "rolling") {                                          
               
       /* uses the local installed kubectl on ci/cd server */
       sh "kubectl set image deployment/helloworld helloworld=jonathanfane/helloworld:${env.BUILD_NUMBER} --kubeconfig=/kubernetes/config/admin.conf"
		
      } else {                                   
       sh "echo hello"
     }

    
	
	}
    
    
}
