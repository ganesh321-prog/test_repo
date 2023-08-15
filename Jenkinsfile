pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
               git credentialsId: 'git_jenkins', url: 'https://github.com/ganesh321-prog/test_repo.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "pwd"
                sh "cd /home/ec2-user"
                sh "mvn clean package"
                sh "mv target/*.jar target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@13.234.118.102:/root/apache-tomcat-9.0.78/webapps/
                    
                    ssh ec2-user@13.234.118.102 /root/apache-tomcat-9.0.78/bin/shutdown.sh
                    
                    ssh ec2-user@13.234.118.102 /root/apache-tomcat-9.0.78/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
