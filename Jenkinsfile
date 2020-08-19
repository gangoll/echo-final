pipeline {
    agent any
    tools {
  terraform 'terraform'
          

    }
    stages {
        stage('pull') {
            steps {
                
                sh 'git pull  || git clone  https://github.com/gangoll/echo-final master'
                sh 'git pull  || git clone  https://github.com/gangoll/echo-final dev'
                sh 'git pull  || git clone  https://github.com/gangoll/echo-final staging'
                script {
                    commit=sh (script: "git log -1 | tail -1", returnStdout: true).trim()
                   
                }
                echo "${commit}"
                
            }
        }          
      
 

        stage('build') { // new container to test
            steps {
                script{
                    sh 'docker network create testing || true'
                    
                        dir('dev'){    
                            sh "docker build -t dev-${GIT_COMMIT_HASH} ." 
                        }
                        dir('master'){    
                            sh "docker build -t 1.0.${JENKINS_BUILD_NUMBER}  ." 
                        }
                        dir('staging'){    
                            sh "docker build -t 'staging-${GIT_COMMIT_HASH}'  ." 
                        }
            
                    }
                }
            }
        
        // stage('test') {
            
        //     steps { 
        //          dir('app'){
        //             script{             //if script returns 1 the job will fail!!
        //                 echo "testing..."
        //                 sh 'chmod +x test.sh || true'
        //                 RESULT=sh (script: './test.sh', returnStdout: true).trim()
        //                 echo "Result: ${RESULT}"
        //              }
        //          }
        //      }
        // }
        
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
    // }

    post {
        always{
            echo 'Removing testing containers:'
            sh "docker rm -f nginx_test || true" //app container
            sh "docker rm -f ted_test || true" 
        }

        success{     
            script{           
               
                
                     #mail $team good worked
                     mail to: "gangoll1992@gmail.com"
                     subject: "${env.JOB_NAME} - (${env.BUILD_NUMBER}) Successfuly",
                     body: "APP building SUCCESSFUL!, see console output at ${env.BUILD_URL} to view the results"
                
            }                
        }
        

        failure{  
            script{   

                
                           mail to: "gangoll1992@gmail.com"
                           subject: "${env.JOB_NAME} - (${env.BUILD_NUMBER}) FAILED",
                           body: "APP building FAIL!, Check console output at ${env.BUILD_URL} to view the results"
                
                
            }
        }
    } 
}