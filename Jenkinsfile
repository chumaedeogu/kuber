pipeline{
    agent{
        any
    }
    stages{
        stage("git check out"){
            steps{
                git branch: 'main', url: 'https://github.com/techporttutor/Addressbookproject.git' 
             }
            stage("maven build"){
            steps{
                sh 'mvn clean build'
             }
           }
    }
}