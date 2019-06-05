pipeline{

agent any

environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus2"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "3.130.67.158:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "my-nexus-snapshots"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "Nexus"
    }

stages{

 stage("Clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/sriharsha219/Hello-World.git';
                }
            }
        }

stage('Compile'){

steps{

echo "Compiling sample maven-webapp"
sh "mvn clean compile"
echo "Project compiled"
    }
}

stage('SonarQube Analysis'){

steps{

echo "Sending code for SonarQube analysis"
sh "mvn sonar:sonar -Dsonar.host.url=http://3.130.69.246 -Dsonar.login=73fb4a283811a21ea3cfe95b9930ba698b5072ab"
echo "Project uploaded to SonarQube"
echo "Check results in SonarQube"
input("Do you want to proceed ?")
    }
}

stage('Test'){

steps{

echo "Testing sample maven-webapp"
sh "mvn clean test"
echo "Project tested"
    }
}
stage('Package'){

steps{

echo "Packing sample maven-webapp"
sh "mvn package"
echo "Project packed"
    }
}
stage('Install'){

steps{

echo "Installing sample maven-webapp"
sh "mvn install"
echo "Project installed"
    }
}

 stage("Publish to Nexus") {
             steps {
				mvn deploy
            }
        }
 
stage ("Deploy-Staging") {		
            steps {
                sh "cd /etc/ansible"
                sh "sudo ansible-playbook nexus-playbook.yml"
       }
   }
}
post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
