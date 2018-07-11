pipeline {
    agent any   
    stages {
    
        stage('Checkout') {
            steps {
                echo "Start checkout project"
                sh 'env'
                step([$class: 'WsCleanup'])
                git url: 'https://github.com/msy53719/maven-jmeter.git', branch: 'master'
                echo 'get artifact from pulugins  pipeline.'
            }
        }
        
        stage('Smoking Test') {
            steps {
                sh 'env'
                echo 'execute test'
                sh ./script/excute_test.sh
            }
        }
        
       stage('reprot name') {
            steps {
                echo 'modiyf report name'
                sh 'pwd'
                sh  ./script/modify_report_name.sh
                sh 'pwd'
            }
        }
    }
    post {
   
        always {
        
         publishHTML target: [
              allowMissing: false,
              alwaysLinkToLastBuild: false,
              keepAll: true,
              reportDir: './target/jmeter/html1/',
              reportFiles: 'index.html',
              reportName: 'Html Report'
            ]
            echo 'package report'
            sh 'sh ./script/report.sh'
            archiveArtifacts artifacts: 'test-report*.tar.gz', fingerprint: true
            emailext attachLog: true, body: '测试报告地址：\n  ${BUILD_URL}/Html_20Report/index.html', compressLog: true, subject: '测试报告地址', to: '479979298@qq.com'
                 
        }
        failure {
            echo 'this area is run when failure'
        }
    }

}

