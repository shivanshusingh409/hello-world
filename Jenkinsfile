pipeline{
    agent any 
    tools{
        maven 'maven-3.9.8'
    }

    stages('Build'){
        steps{
            sh 'mvn clean package'
        }
        post{
            success{
                echo"Archiving the Artifacts"
                archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }

    stage ('Deploy to Tomcat server '){
        steps{
            deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer', path: '', url: 'http://52.87.177.82:8080/')], contextPath: null, war: '**/*.war'
        }
    }
}
