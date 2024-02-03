pipeline {   
  agent any
  environment {     
    DOCKERHUB_CREDENTIALS= credentials('docker-hub')     
  }    
  stages {         
    stage("Git Checkout"){           
      steps{                
        git credentialsId: 'github', url: 'https://github.com/tamartayar/personal-project.git/'                 
        echo 'Git Checkout Completed' 
      }        
    }
    stage("build"){           
      steps{                
        sh '''
        cd super-app
          docker compose build --no-cache
        docker tag tamartayar/personal-project:node tamartayar/personal-project:node.$BUILD_NUMBER
        docker tag tamartayar/personal-project:php tamartayar/personal-project:php.$BUILD_NUMBER
        '''
      }        
    }
    stage("run"){           
      steps{                
        sh '''
        cd super-app
        docker compose up -d
        sleep  3
        '''
      }        
    }
    stage("tests"){           
      steps{                
        sh '''
        cd super-app
        curl localhost:8088
        curl localhost:3000/super-app
        '''
      }        
    }
    stage('Login to Docker Hub') {         
      steps{
        sh '''
        docker login -u $DOCKERHUB_CREDENTIALS_USR -p$DOCKERHUB_CREDENTIALS_PSW                 
        echo Login Completed             
        '''
       }           
    }
    stage('Push Image to Docker Hub') {         
      steps{                            
        sh '''
        docker push tamartayar/personal-project:node.$BUILD_NUMBER
        docker push tamartayar/personal-project:php.$BUILD_NUMBER
        echo Push Image Completed
        '''
      }           
    } 
  }
  post{
    always {  
      sh 'docker logout'           
    }      
  }              
}
