pipeline {
    agent any
          environment {
    registry = "gangoll/test"
    registryCredential = 'dockerhub'
}
    triggers {
        githubPush()
    }


    stages {
        stage('pull') {
            steps {
                script{
                if (BRANCH_NAME =~ /^(master)/){
                    dir('master'){  
                sh 'git pull  || git clone --single-branch --branch master  https://github.com/gangoll/echo-final'}}

                if (BRANCH_NAME =~ /^(dev)/){
                    dir('master'){  
                sh 'git pull || git clone --single-branch --branch dev https://github.com/gangoll/echo-final'}}
                if (BRANCH_NAME =~ /^(staging)/){
                    dir('master'){  
                sh 'git pull || git clone --single-branch --branch staging  https://github.com/gangoll/echo-final'}}
            }
                script {
                    GIT_COMMIT_HASH=sh (script: "git log -1 | tail -1", returnStdout: true).trim()
                   
                }
                echo "${GIT_COMMIT_HASH}"
                
            }
        }          
      
        stage('build') { // new container to test
            steps {
                script{
                        if (BRANCH_NAME =~ /^(dev)/){
                        dir('dev'){    
                            sh "docker build -t dev-${env.GIT_COMMIT} ." 
                        }}
                        if (BRANCH_NAME =~ /^(master)/){
                        dir('master'){    
                            sh "docker build -t 1.0.${env.BUILD_NUMBER} ." 
                        }}
                        if (BRANCH_NAME =~ /^(staging)/){
                        dir('staging'){    
                            sh "docker build -t 'staging-${env.GIT_COMMIT}'  ." 
                        }}
            

                    }
                }
            }
        
        stage('push to github') {
            
            steps { 
                 
                 
                    script{             //if script returns 1 the job will fail!!
                         docker.withRegistry( '', registryCredential ){
                      if (BRANCH_NAME =~ /^(master)/){
                           sh "docker tag 1.0.${env.BUILD_NUMBER} gangoll/1.0.${env._BUILD_NUMBER} && docker push gangoll/1.0.${env.BUILD_NUMBER}"
                      }
                       if (BRANCH_NAME =~ /^(dev)/){
                        docker.withRegistry( '', registryCredential ){
                        sh "docker tag dev-${env.GIT_COMMIT} gangoll/dev-${env.GIT_COMMIT} && docker push gangoll/dev-${env.GIT_COMMIT}"}
                       
                        if (BRANCH_NAME =~ /^(staging)/){
                        sh "docker tag staging-${env.GIT_COMMIT} gangoll/staging-${env.GIT_COMMIT} && docker push gangoll/staging-${env.GIT_COMMIT}"}
                        
                     }
                 
             }}
        }}
        
        // stage('deply')
        // {

        // when {
        //             expression {BRANCH_NAME =~ /^(master$| release\/*)/ || commit == "test"
        //             }
        // }
        // steps
        // {
        
        //   script{             //if script returns 1 the job will fail!!
        //                 echo "depploying..."
        //                 sh "terraform init || true"
        //                sh "terraform destroy -auto-approve || true"
        //                sh "terraform apply -auto-approve"
                        
        //              if ("${commit}" == "test"){
        //                 sh '''
        //                 sed -i "s/localhost:8080/$(head -1 to-replace)/g" test.sh
        //                  ./test.sh
        //                  '''
        //                 }
    //                      }
    //     }
    //     }
    }

    // post {
    //     always{
    //         // echo 'Removing testing containers:'
    //         // sh "docker rm -f nginx_test || true" //app container
    //         // sh "docker rm -f ted_test || true" 
    //     }

    //     success{     
    //         script{           
               
                
                   
    //                 //  mail to: "gangoll1992@gmail.com"
    //                 //  subject: "${env.JOB_NAME} - (${env.BUILD_NUMBER}) Successfuly",
    //                 //  body: "APP building SUCCESSFUL!, see console output at ${env.BUILD_URL} to view the results"
                
    //         }                
    //     }
        

    //     failure{  
    //         script{   

                
    //                     //    mail to: "gangoll1992@gmail.com"
    //                     //    subject: "${env.JOB_NAME} - (${env.BUILD_NUMBER}) FAILED",
    //                     //    body: "APP building FAIL!, Check console output at ${env.BUILD_URL} to view the results"
                
                
    //         }
    //     }
    // } 
}
