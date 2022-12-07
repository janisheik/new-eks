pipeline {
    agent any
    environment {
        registry = "463702037504.dkr.ecr.us-east-2.amazonaws.com/public.ecr.aws/c9e3o3h3/nginx"
    }
   
    stages {
         stage ('git clone') {
            steps {
        echo "code is building"
         git 'https://github.com/janisheik/new-eks.git'
            }
        }

  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 463702037504.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker tag nginx:latest public.ecr.aws/c9e3o3h3/nginx:latest'
                sh 'docker push public.ecr.aws/c9e3o3h3/nginx:latest'
         }
        }
      }

      
        stage('kubectl deploy'){ 
       steps
        {
          sh 'sudo kubectl apply -f httpd-dep.yaml'
          sh 'sudo kubectl get nodes'
          sh 'sudo kubectl get svc'
          sh 'sudo kubectl get ns'
          sh 'sudo kubectl get svc'
          sh 'sudo kubectl rollout restart deployment/httpd-deployment'
           
        }
      } 
    }
}
