pipeline {
    agent any
    stages {
        stage('Setup Local Environment') {
            steps {
                echo '--RUNNING LOCAL ENVIRONMENT --'
                sh '''#!/bin/bash
                sudo apt-get update
                sudo apt-get install -y python3-dev libffi-dev gcc libssl-dev docker.io
                sudo apt install -y python3-pip
                sudo apt install -y python3.10-venv
                sudo python3 -m venv local
                source local/bin/activate
                '''
            }
        }
        stage('INSTALLING PIP') {
            steps {
                echo '--INSTALLING PIP --'
                sh '''#!/bin/bash
                pip install -U pip
                '''
            }
        }
        stage('INSTALLING Ansible') {
            steps {
                echo '--INSTALLING Ansible --'
                sh '''#!/bin/bash
                pip install 'ansible-core>=2.16,<2.17.99'
                '''
            }
        }
        stage('INSTALLING Kolla Ansible') {
            steps {
                echo '--INSTALLING Kolla Ansible --'
                sh '''#!/bin/bash
                pip install pip install git+https://opendev.org/openstack/kolla-ansible@master
                kolla-ansible install-deps
                '''
            }
        }
        stage('Preparing Infrastructure') {
            steps {
                echo '--Preparing Infrastructure Files Structure --'
                sh '''#!/bin/bash
                sudo mkdir -p /etc/kolla
                sudo chown $USER:$USER /etc/kolla
                cp -r /usr/local/share/kolla-ansible/etc/kolla/* /etc/kolla/
                cp -r /usr/local/share/kolla-ansible/ansible/inventory/* /etc/kolla/
                '''
            }
        }
        stage('Secrets Setup') {
            steps {
                echo '--Generating OpenStack Services Secrets --'
                sh '''#!/bin/bash
                kolla-genpwd -p /etc/kolla/passwords.yml
                '''
            }
        }
        stage('Bootstrap Servers') {
            steps {
                echo '--Running Ansible Kolla Bootstrap Server Script --'
                sh '''#!/bin/bash
                kolla-ansible -i /etc/kolla/multi_packtpub_prod bootstrap-servers
                '''
            }
        }
        stage('Infrastructure Pre-Checks') {
            steps {
                echo '--Running Ansible Kolla Prechecks Script --'
                sh '''#!/bin/bash
                kolla-ansible -i /etc/kolla/multi_packtpub_prod prechecks
                '''
            }
        }
        stage('Deploy Infrastructure') {
            steps {
                echo '--Running Ansible Kolla Deploy Script --'
                sh '''#!/bin/bash
                kolla-ansible -i /etc/kolla/multi_packtpub_prod deploy
                '''
            }
        }
    }
}
