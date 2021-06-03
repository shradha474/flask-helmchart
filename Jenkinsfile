node {    
      def flaskapp     
      stage('Clone repository') {               
             
            checkout scm    
      }     
      stage('Build image') {         
       
            flaskapp = docker.build("{{yourdockerid}}/flask-sezzle")    
       }     
      stage('check image') {           
            flaskapp.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        }     
       stage('Push image') {
                                                  docker.withRegistry('https://registry.hub.docker.com', 'git') {            
       flaskapp.push("${env.BUILD_NUMBER}")            
       flaskapp.push("latest")        
              }    
           }
        stage('Deploy new image'){
          script {
            sh 'helm install flaskapp flaskapp/ --values flaskapp/values.yaml'
          }        
        }
      }
