#!groovy

node {
    currentBuild.result = "SUCCESS"

    try {

       stage('Checkout'){

          checkout scm
       }
	stage('Sonar') {
               //add stage sonar
                sh 'mvn sonar:sonar'
             }

       stage('Compiling'){

          sh 'mvn clean deploy'
       }
	   
	    
	stage('Checkstyle') {
                    sh 'mvn checkstyle:checkstyle'
                }

               stage('PMD') {
                    sh 'mvn pmd:check'
                }
      /* stage('mail'){

         mail body: 'project build successful',
                     from: 'devopstrainingblr@gmail.com',
                     replyTo: 'mithunreddytechnologies@gmail.com',
                     subject: 'project build successful',
                     to: 'mithunreddytechnologies@gmail.com'
       }*/
	    stage('Deploy-To-Tomcat'){
             sshagent(['tomcat']) {
             sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@10.10.2.254:opt/apache-tomcat-8.0.53/webapps'
       }
    }
    catch (err) {
         currentBuild.result = "FAILURE"
	    /* mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'devopstrainingblr@gmail.com',
            replyTo: 'mithunreddytechnologies@gmail.com',
            subject: 'project build failed',
            to: 'mithunreddytechnologies@gmail.com'
            */
        throw err
    }
}
