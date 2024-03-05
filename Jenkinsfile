pipeline {
   environment {
     git_url = "https://github.com/salilkul/java-project.git"
     git_branch = "master"
   }

  agent {label 'dev'}
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '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', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build1') {
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
                 withCredentials([usernamePassword(credentialsId: '43d1ce2d-613e-4cea-8767-469eeefb4c4d', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image varunroman/myjava-image:test"
                 sh "sudo docker image push varunroman/myjava-image:test" 
               } 
             }  
          }
      //stage('Deploy app') {
        // steps {
        //    sh 'kubectl apply -f app-deploy.yaml'
         }
      }
    }

//  post {
   // always {
     // deleteDir() /* cleanup the workspace */
    }
  }
  }
