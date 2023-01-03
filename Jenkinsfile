def dev_env='192.168.100.115';
def qa_env='192.168.100.81';
pipeline {
    agent {label 'DEV'}

    stages {
        stage('clone') {
            steps {
                dir('backend'){
                   git branch: 'master', url: 'https://github.com/jhossmar/MusicShopDjango.git'
                   
                }
                dir('frontend'){
                    git branch: 'main', url: 'https://github.com/jhossmar/proyecto-final-vue.git'
                    
                }
            }
        }
        stage ('docker build'){
            steps{
               dir('backend'){
                sh "docker build -t musicshopdjango:1.0 ."

               }
               dir('frontend'){
                sh "docker build -t proyecto_vue/final:v1  ."
               }
            }
        }
        stage('Deploy  DEV'){
            steps{
                sh "mkdir -p /home/marcelo/Final"
                dir ('/home/marcelo/Final'){
                   cleanWs()
                   git branch: 'main', url: 'https://github.com/jhossmar/jenkinsfiles.git' 
                   sh "docker-compose rm -sf"
                   sh "docker-compose up -d"
                }
            }
        }
        stage('Testing DEV'){
            steps{
                sh "curl -v http://${dev_env}:80/"
                sh "curl -v http://${dev_env}:8000/ | true" 
            }

        }
        stage('Docker save'){
            steps{
                sh "docker save proyecto_vue/final:v1 | gzip > frontend.tar.gz"
                sh "docker save musicshopdjango:1.0 | gzip > backend.tar.gz"
                stash name: 'backend', includes: 'backend.tar.gz'
                stash name: 'frontend', includes: 'frontend.tar.gz'
                  
            }
        }
        stage('DeployQA'){
            agent{label 'debian'}
            steps{
                sh "mkdir -p /home/debian/Final"
                dir ('/home/debian/Final'){
                   cleanWs()
                   git branch: 'main', url: 'https://github.com/jhossmar/jenkinsfiles.git' 
                   sh "docker-compose rm -sf"
                   sh "echo 'Y' | docker image prune -a"
                   unstash 'backend'
                   unstash 'frontend'
                   sh "docker load -i backend.tar.gz"
                   sh "docker load -i frontend.tar.gz"
                   sh "docker-compose up -d"
                }
            }

        }
        stage('Testing QA'){
            steps{
                sh "curl -v http://${qa_env}:80/"
                sh "curl -v http://${qa_env}:8000/ | true" 
            }

        }
        stage('Deploy PROD'){
            agent{label 'PROD'}
            steps{
                sh "mkdir -p /home/ubuntu/Final"
                dir ('/home/ubuntu/Final'){
                   cleanWs()
                   git branch: 'main', url: 'https://github.com/jhossmar/jenkinsfiles.git' 
                   sh "docker-compose rm -sf"
                   sh "echo 'Y' | docker image prune -a"
                   unstash 'backend'
                   unstash 'frontend'
                   sh "docker load -i backend.tar.gz"
                   sh "docker load -i frontend.tar.gz"
                   sh "docker-compose up -d"
                }


            }

        }
    }
}
