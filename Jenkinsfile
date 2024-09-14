pipeline {
    agent any

    stages {
          stage('code compile') {
            steps {
                echo 'compile code'
                sh '/opt/maven/bin/mvn compile'
            }
        }
         stage('metrics') {
            steps {
                echo 'metrics'
                sh '/opt/maven/bin/mvn -P metrics pmd:pmd'
            }
           post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }	
        }
        
          stage('test') {
            steps {
                echo 'test'
                sh '/opt/maven/bin/mvn test'
            }
            post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }	
        }
        
        stage('codecoverage') {
	   steps {
                echo 'unittest..'
	        sh script: '/opt/maven/bin/mvn verify'
                 }
	   post {
               success {
                   jacoco buildOverBuild: true, deltaBranchCoverage: '20', deltaClassCoverage: '20', deltaComplexityCoverage: '20', deltaInstructionCoverage: '20', deltaLineCoverage: '20', deltaMethodCoverage: '20'
               }
           }			
        }
        
          stage('package') {
            steps {
                echo 'package'
                sh '/opt/maven/bin/mvn package'
            }
        }
    }
}
