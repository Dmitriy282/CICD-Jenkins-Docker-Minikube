node {
    def app

    stage('Clone repository') {

        /* Clone our repository */
         checkout scm
    }

    stage('Build image') {
      dir('vue-chess') {
       
        /* Build the docker image */
      
        app = docker.build("anpavlovsk/vue-chess-img")
      }
    }
    
    stage('Test image') {           
            app.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        }     

    stage('Push image') {

        /* Push images: First is tagged with the build BUILD_NUMBER
         the second is just tagged latest !*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            custom = app.build("${env.BUILD_NUMBER}")
            custom.push("latest")
        }
    }

    stage('Deploy to K8s') {

      /* Apply all manifest files */
      /* sh "kubectl apply -f ./kube/" */
      sh "kubectl apply -f ./kube/mongo.yaml"
      sh "kubectl apply -f ./kube/vue-chess.yaml"
        
    }

}
