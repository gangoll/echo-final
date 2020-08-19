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
                
                sh 'git pull || git clone --single-branch --branch master  https://github.com/gangoll/echo-final ./master'
                sh 'git pull || git clone --single-branch --branch dev https://github.com/gangoll/echo-final ./dev'
                sh 'git pull || git clone --single-branch --branch staging  https://github.com/gangoll/echo-final ./staging'
                script {
                    GIT_COMMIT_HASH=sh (script: "git log -1 | tail -1", returnStdout: true).trim()
                   
                }
                echo "${GIT_COMMIT_HASH}"
                
            }
        }          
      
        stage('build') { // new container to test
            steps {
                script{
                    
                        dir('dev'){    
                            sh "docker build -t dev-${GIT_COMMIT_HASH} ." 
                        }
                        dir('master'){    
                            sh "docker build -t 1.0.${env.JENKINS_BUILD_NUMBER}  ." 
                        }
                        dir('staging'){    
                            sh "docker build -t 'staging-${GIT_COMMIT_HASH}'  ." 
                        }
            

                    }
                }
            }
        
        stage('push to github') {
            
            steps { 
                 
                    script{             //if script returns 1 the job will fail!!
                         
                      if (BRANCH_NAME =~ /^(master)/){
                           sh "docker tag 1.0.${env.JENKINS_BUILD_NUMBER} gangoll/1.0.${env.JENKINS_BUILD_NUMBER} && docker push gangoll/1.0.${env.JENKINS_BUILD_NUMBER}"
                      }
                       if (BRANCH_NAME =~ /^(dev)/){
                        docker.withRegistry( '', registryCredential ){
                        sh "docker tag dev-${GIT_COMMIT_HASH} gangoll/dev-${GIT_COMMIT_HASH} && docker push gangoll/dev-${GIT_COMMIT_HASH}"}
                       
                        if (BRANCH_NAME =~ /^(staging)/){
                        sh "docker tag staging-${GIT_COMMIT_HASH} gangoll/staging-${GIT_COMMIT_HASH} && docker push gangoll/staging-${GIT_COMMIT_HASH}"}
                        
                     }
                 
             }
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
