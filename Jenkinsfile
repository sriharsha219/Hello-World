pipeline{

agent any

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
