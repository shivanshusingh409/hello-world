def register = 'https://shivanshu3003.jfrog.io/'
pipeline{
    agent any 
    tools{
        maven 'maven-3.9.8'
    }
    stages{

    stage('Build'){
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
        stage ('Publish war to repository'){
            steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-pass"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "webapp/(*)",
                              "target": "mvn-libs-snapshot-local/{1}",
                              "flat": "false",
                              "props" : "${properties}"
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
                }
            }  
        }

    stage ('Deploy to Tomcat server '){
        steps{
            deploy adapters: [tomcat9(credentialsId: 'tomcat_main_deployer', path: '', url: 'http://52.87.177.82:8080/')], contextPath: null, war: '**/*.war'
        }
    }
}
}
