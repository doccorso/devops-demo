pipeline {
    agent any
    tools { 
        maven 'M2_HOME' 
        jdk 'JAVA_HOME' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage('SCM Checkout'){
            steps {
                echo 'Checking out project on Git Repo'
                git 'https://github.com/doccorso/devops-demo.git'
            }
        }
        
        stage ('Compile Stage') {
            steps {
                echo 'Build Compile project'
                sh 'mvn clean compile'
                echo 'Build Compile Successful'
            }
        }
        
        
        stage ('Build') {
            steps {
                echo 'Build Install project'
                sh 'mvn install'
                echo 'Build Install Successful'
            }
        }
        
        
        stage ('Deploy to Kubernetes'){
            steps {
                echo 'Deploy project on Kubernetes cluster'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/create-devops-image.yml;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//kubernetes', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                
                echo 'Deploy on Kubernetes Successful'
            }
                
        }
        
        stage('Kubernetes deployment and service') {
            steps {
                echo 'Create Kubernetes deployment and service with Ansible'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/kubernetes-doccorso-deployment.yml;
    ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/kubernetes-doccorso-service.yml;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
