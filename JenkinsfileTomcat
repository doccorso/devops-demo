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
        
        
        stage ('Deploy to Tomcat'){
            steps {
                echo 'Deploy project on Tomcat Server'
                deploy adapters: [
                    tomcat8(credentialsId: 'deployer_user', 
                    path: '', url: 'http://18.188.44.224:8080/')], contextPath: null, war: '**/*.war'
                
                echo 'Deploy on Tomcat Server Successful'
            }
                
        
    }
}

}
