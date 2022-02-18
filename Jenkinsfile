pipeline {

    agent any

    tools {
        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "Maven 3.8.4"
    }

    
    stages {
        stage("clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/syju/maven_repo.git';
                }
            }
        }

        stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh script: 'mvn clean package'
                    archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
                }
            }
        }

        stage("publish to nexus") {
            steps {
              script{
                def mavenPom = readMavenPom file: 'pom.xml'
                def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'simple-app',
                        classifier: '', 
                        file: 'target/simple-app-3.0.0-SNAPSHOT.war',
                        type: 'war'
                    ]
                ], 
                credentialsId: 'nexus-credentials', 
                groupId: 'in.javahome', 
                nexusUrl: '18.236.183.77:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'maven-nexus-repo', 
                version: '${mavenPom.version}'

                }
                    
            }
        }

    }
}
