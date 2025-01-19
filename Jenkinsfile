pipeline {
    agent any
    stages{
        stage('Git checkout'){
            steps
            {
                echo 'Checking out the git repo...'
                git url:' https://github.com/Balasastha/Project-3.git'
            }
        }
        stage('Package the code'){
            steps
            {
                echo 'Packaging the code for you...'
                sh 'mvn clean package'
            }
        }
        stage ('Build docker image'){
            steps
            {
                echo 'Building docker image for you..'
                sh 'docker build -t balasastha/healthcare-domain:3 .'
            }
        }
        stage ('Docker login'){
            steps
            {
                echo 'Loging in into docker...'
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                sh 'docker login -u balasastha -p ${dockerhubpass}'
                }
            }
        }
        stage ('Push image into docker'){
            steps
            {
                echo 'Pushing the docker image for you'
                sh 'docker push balasastha/healthcare-domain:3'
            }
        }
        stage ('Invoking ansible playbook'){
            steps
            {
                echo 'Running ansible playbook in test server...'
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
        stage ('Deployment of project on production server'){
            steps
            {
                echo 'Deploying using kubernetes..'
                sh 'sudo kubectl apply -f deployment.yaml'
                sh 'sudo kubectl get all'
                
            }
        }
    }	
}

