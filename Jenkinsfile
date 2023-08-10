pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "master"
   }

  agent {label 'dev'}
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: 'de69b7c2-55e9-4591-ba5b-5806fc84c864', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build') {
     steps { 
          sh "mvn clean package && cp target/*.jar . "
     }
    }
     
     stage('Docker Image Build') {     
        steps {
              sh 'sudo docker build -t myjava-image . '
               }
             }
        stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: '59cdb4e2-04b4-46fb-a858-d01def4ad7dc', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image varunroman/myjava-image:test"
                 sh "sudo docker image push varunroman/myjava-image:test" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
            sh 'sudo kubectl apply -f app-deploy.yaml'
         }
      }
    }

  post {
    always {
      deleteDir() /* cleanup the workspace */
    }
  }
  }
