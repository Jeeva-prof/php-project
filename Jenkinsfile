pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/Jeeva-prof/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t 10551jeeva/php:v1 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push 10551jeeva/php:v1'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f con-php || true'
                    def dockerCmd = 'sudo docker run -itd --name con-php -p 8083:80 10551jeeva/akshatnewimg6july:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 10551jeeva/php:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@13.234.115.28 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@13.234.115.28 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
