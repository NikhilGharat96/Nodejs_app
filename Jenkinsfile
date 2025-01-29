pipeline {
    agent any
    tools {
        nodejs 'mynode'
    }
    stages {
        stage('git cloning') {
            steps {
                echo 'cloning files from github'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NikhilGharat96/Nodejs_app.git']])

            }
        }
        stage('Build') {
            steps {
                echo 'Building nodejs project'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing project'
                sh "./node_modules/mocha/bin/_mocha --exit ./test/test.js"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying nodejs project on live server'
                script{
                    sshagent(['cb6a638a-8c25-4c8e-9abe-f460921fd41b']) {  
                     sh '''
                           ssh -o StrictHostKeyChecking=no ubuntu@44.202.33.169<<EOF                         // Add your liveserver IP 
                            cd /home/ubuntu/nodeapp/
                            git pull https://github.com/NikhilGharat96/Nodejs_app.git
                            npm install
                            sudo npm install -g pm2
                            pm2 restart index.js || pm2 start index.js
		                    exit
                            EOF     
                        '''
                        }
                    }
                }
            }
        }
    }
