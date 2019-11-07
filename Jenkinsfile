pipeline {
    agent any
    stages {
        stage ('Build Servlet Project') {
            steps {
                /*For windows machine */
               sh  'mvn clean package'

                /*For Mac & Linux machine */
               // sh  'mvn clean package'
            }

            post{
                success{
                    echo 'Now Archiving ....'

                    archiveArtifacts artifacts : '**/*.war'
                }
            }
        }
        stage ('Deploy Build in staging area') {
            steps {
                build job: 'deploy_stagingArea_pipeline'
            }
        }
        stage ('Deploy to Production') {
            steps{
                timeout (time: 5, unit: 'DAYS'){
                    input message: 'Approve Production Deployment?'
                }
                build job : 'Deploy_production_Pipeline'
            }

            post{
                success{
                    echo 'Deployment on Production is successful'
                }

                failure{
                    echo 'Deployment Failure on Production'
                }
            }
        }
    }
}
