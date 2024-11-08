pipeline{
    agent {label 'docker'}
    tools {
        maven 'maven'
          }
    environment {
        SONAR_HOME = tool "sonar-scanner"
    }
    stages{
        stage("clean work space"){
            steps{
                cleanWs()
            }
        }
    stage("check out"){
        steps{
            git branch: 'test', url: 'https://github.com/chumaedeogu/hibernate5.git'
        }
    }

   stage("unit test"){
        steps{
            sh 'mvn clean test'
        }
    }
    stage("static analysis"){
        steps{
              withSonarQubeEnv('SonarQubeServer') {
                sh '''
                   ${SONAR_HOME}/bin/sonar-scanner -Dsonar.projectName="gitest"\
                   -Dsonar.projectKey="gitest" \
                   -Dsonar.java.binaries=.
                   '''
   
               }
            
        }
    }
    stage("wait for quilty gate"){
        steps{
            timeout(time: 6, unit: "MINUTES"){
                waitForQualityGate abortPipeline: false, credentialsId: 'SonarQubeServer'
            }
        }
    }
    stage("owasp check"){
        steps{
             dependencyCheck additionalArguments: '''
             -o target/
              -s target/
              -f ALL
            ''', odcInstallation: 'owasp'
        }
    }
    stage("create artifacts"){
        steps{
            sh ' mvn package -DskipTest=True'
        }
    }
    stage("docker build"){
        steps{
            sh 'docker build -t chumaedeogu/java-test .'
        }
    }
    stage("trivy check"){
        steps{

        
        sh 'trivy image java-test --severity HIGH,CRITICAL --exit-code 0'
        }
    }
    }
}


    