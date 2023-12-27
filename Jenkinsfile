pipeline {
    agent {
        label {
            label "built-in"
            
        }
        
    }
    stages {
        stage ("clean-workspace") {
            steps {
                sh "rm -rf /root/.jenkins/*"
                
            }
            
        }
        stage ("clone-repo") {
            steps {
                git 'https://github.com/Shantanumajan6/project.git'
                
            }
            
        }
        stage ("clean-workspace") {
            steps {
                sh "rm -rf /root/.m2/*"
                
            }
            
        }
        stage ("build") {
            steps {
                sh "mvn clean install"
                
            }
            
        }
        stage ("copy-to-slave") {
            agent {
                label {
                    label "slave-1"
                    
                }
                
            }
            steps {
                sh "scp /root/.jenkins/workspace/project/target/loginapp.war ec2-user@172.31.14.10:/home/ec2-user/"
                
            }
            
        }
       stage ("install-docker") {
           agent {
               label {
                   label "slave-1"
                   
               }
               
           }
           steps {
               sh "chmod -R 777 /home/ec2-user"
               sh "sudo yum install docker -y"
               sh "sudo systemctl start docker"
               sh "sudo systemctl enable docker"
               sh "sudo docker run -itdp 8001:80 --name container1 tomcat bash"
               sh "sudo docker cp /home/ec2-user/loginapp.war container1:/usr/local/webapps/"
               
           }
           
       }
            
        
    }
    
}
