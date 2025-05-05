pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-org/k8s-ansible-aws.git'
            }
        }
        stage('Provision AWS Infrastructure') {
            steps {
                sh '''
                    ansible-playbook -i inventories/aws_ec2.yml playbooks/provision.yml
                '''
            }
        }
        stage('Install Kubernetes on Master') {
            steps {
                sh '''
                    ansible-playbook -i inventories/aws_ec2.yml playbooks/install_k8s.yml
                '''
            }
        }
        stage('Join Worker Nodes') {
            steps {
                sh '''
                    ansible-playbook -i inventories/aws_ec2.yml playbooks/join_nodes.yml
                '''
            }
        }
    }
}
