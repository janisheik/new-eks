pipeline {
    agent any

    stages {
         stage ('git clone') {
            steps {
        echo "code is building"
         git 'https://github.com/janisheik/new-eks.git'
            }
        }
        
        stage('build docker docker image') {
            steps {
                echo "buid docker image"
                
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/c9e3o3h3'
                sh 'docker build -t nginx .'
                sh 'docker tag nginx:latest public.ecr.aws/c9e3o3h3/nginx:1.23'
                sh 'docker push  public.ecr.aws/c9e3o3h3/nginx:1.23'
               
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
