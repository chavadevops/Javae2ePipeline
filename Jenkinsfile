pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
   
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/chavadevops/Javae2ePipeline'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
        
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins_sonar') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }

        stage("Quality Gate") {
            steps {
                script {
                   // waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonar'
                    timeout(time: 1, unit: 'HOURS'){
                    def qg = waitForQualityGate()
                        if (qg.status != 'OK'){
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }


        } 

    }
        }

  
}
