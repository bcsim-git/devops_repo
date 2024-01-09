pipeline {
      agent any
      stages {
          stage('One') {
          steps {
            echo 'Continue from Release Phase'
          }
          }
          stage('Two') {
          steps {
                 echo "Proceed to Deploy Phase - Deploy to QA environment"                 
		 sh '''#!/bin/bash
                 puppet resource file /tmp/clone ensure=absent force=true;
                 puppet resource file /tmp/clone ensure=directory;
	         cd /tmp/clone;
	         git clone https://github.com/bcsim-git/devops_repo.git;
                 target1='puppetclient1.localdomain';
                 locate_script='/tmp/clone/devops_repo/script_to_run';
                 bolt script run $locate_script -t $target1 -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "QA container updated"
          }
          }
	  stage('Three') {
          steps {
                  echo 'QA Test - Begin ......'
		  echo 'QA Test - QA Report Generated'
          }
          }
          stage('Four') {
            steps {
                script {
                v2 = input ( 
                       message: 'Action',
                       parameters: [choice(name:'',choices: ['Deploy to Production Environment', 'QA Environment Rollback'])]
                            )
                       }
	           }
	    }          
          stage('Five') {
             steps {
                script {
                   if (v2 == 'QA Environment Rollback') {
                   echo 'Rollback of QA container Completed'
                   } else if (v2 == 'Deploy to Production Environment') {
                   sh '''#!/bin/bash
                   target2='puppetclient2.localdomain';
                   locate_script='/tmp/clone/devops_repo/script_to_run';
                   bolt script run $locate_script -t $target2 -u clientadm -p user123 --no-host-key-check --run-as root;
                   '''
		   echo "Production container updated"
		   }	      
		 }
	     }
	  }
	  stage('Six') {
          steps {
                  echo 'Begin of Operate Phase'
		  sh ''' #!/bin/bash
                  v3 = `curl -IL http://$target2 |grep "OK" |wc -l`
		  echo "v3 value is $v3" 
		  '''
	         }
	   steps {
		  if (v3 == 1) {
                  echo 'Website is running .....'
		  } else if (v3 == 0) {
                  echo 'Website is running .... Trigger Notification'
		  }
	          }
          }
      }
}

