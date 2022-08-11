pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                
                git 'https://github.com/krishnaveni-byte/mydemo.git'

               
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                
            }
        }

            stage("build docker image"){
            steps{
                script{
                    sh 'docker version'
                    
                    sh "docker build -t tomcat:8.0.53 ."
            
                }
            }
        }
        stage('push docker image'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'newkey', variable: 'password')]) {
                        sh "docker login -u my321docker321 -p $password"
                        sh "docker tag tomcat:8.0.53 my321docker321/tomcat:8.0.53 "
                   
                        sh 'docker push my321docker321/tomcat:8.0.53 '
                       sh 'aws eks update-kubeconfig --name my-test-cluster'

                    }
                }
            }
        }
        
        stage('kubernetes'){
                      steps{
                        script{
                            sh ' kubectl delete -f depolyment.yml || true'
                            sh 'kubectl apply -f depolyment.yml '
                        } 
                      }
                  } 
        
    }
}
