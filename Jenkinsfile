
pipeline {
    parameters {
  choice choices: ['dev', 'stg', 'master'], description: 'pls select your branch', name: 'Branch'
}
    agent any
environment {
  PATH = "/opt/maven/bin:$PATH"
}
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('checkout'){
             steps{
               checkout(
[
$class: 'GitSCM', 
branches: [
    [name: '*/${Branch}']], 
extensions: [], 
userRemoteConfigs: [
[credentialsId: 'githubcred', 
url: 'https://github.com/bharathpsv6/Hello-world.git']
]
]
)
}
}
stage('build'){
steps{
sh 'mvn package'
}
}
stage('Artifact upload'){
steps{
nexusArtifactUploader artifacts: [
[artifactId: 
'hello-world-war', 
classifier: '', 
file: 
'target/hello-world-war-3.0.1.war', 
type: 'war']
], 
credentialsId: 'nexus-cred', 
groupId: 'com.efsavage', 
nexusUrl: '3.110.127.95:8081', 
nexusVersion: 'nexus3', 
protocol: 'http', 
repository: 'Myrepo', 
version: ' 3.0.1 '
}
}
stage('deploy'){
    steps{
        deploy adapters: 
            [
                tomcat8(
                    credentialsId: 'Tomcat-credentials', 
                    path: '', url: 'http://13.235.135.73:8080'
                )
            ], 
            contextPath: 'dev', war: '**/*.war'
    }
}
        }
}
