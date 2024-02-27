pipeline {
    agent any
    
    tools {
        jdk "jdk17"
        maven "M3"
    }


    stages {
        stage('Git Clone') {
            steps {
                echo 'Git Clone'
                git url: 'https://github.com/sjh4616/spring-petclinic.git', branch: 'wavefront', credentialsId: 'github_access_token'
            }
        }
        stage('Build') {
            steps{
                echo 'Build'
                sh 'mvn -Dmaven.test.failure.ignore=true clean package'
            }
        }
        stage('SSH Publish') {
            steps{
                echo 'SSH Publish'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'target',
                transfers: [sshTransfer(cleanRemote: false,
                excludes: '',
                execCommand: '''
                fuser -k 8080/tcp
                export BUILD_ID=Pipeline-Test
                nohup java -jar /home/ubuntu/deploy/spring-petclinic-2.4.0.BUILD-SNAPSHOT.jar >> nohup.out 2>&1 &''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'deploy', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/*.jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
