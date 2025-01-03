pipeline {
    agent any
    environment {
       // AWS_REGION = 'us-east-1'
        MAVEN_HOME = '/usr/share/maven'  // maven home directory.  Obtain home directory using mvn --version
    }
    stages {
        stage('Checkout Code') {
            steps {
               // replace git URL below with your git repo url
                git branch: 'main', url: 'https://github.com/mecbob/jenkins-cicd.git'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('JJtechBatchApp'){
                sh "${MAVEN_HOME}/bin/mvn clean compile test package"
            }
         }
        }

        stage('Scan') {
            steps {
                 dir('JJtechBatchApp') {
                 withSonarQubeEnv(installationName: 'jenkins-sonar') { 
                    // sh './mvn clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'   To use a specific version of sonarqube
                    sh "${MAVEN_HOME}/bin/mvn clean sonar:sonar"       // uses the installed sonar plugin
                 } }
            }
        }



        stage("Quality Gate") {       // for quality gates, the pipeline will wait for sonar to send back a success quality gate check. 
             steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }


        




        // stage('Initialize Terraform') {
        //     steps {
        //         sh 'terraform init'
        //     }
        // }
        // stage('Plan Terraform') {
        //     steps {
        //         sh 'terraform plan -out=tfplan'
        //     }
        // }
        // stage('Apply Terraform') {
        //     steps {
        //         input message: "Approve deployment?", ok: "Deploy"
        //         sh 'terraform apply tfplan'
        //     }
        // }
    }
}