pipeline {
      agent any
      stages {
          stage('One') {
          steps {
            echo 'Begin of Pipeline: Stage one completes'
          }
          }
          stage('Two') {
          steps {
            input('Do you want to proceed to QA environment?')
          }
          }
          stage('Three') {
          when {
                not {
                    branch "Development NOT updated"
                }
          }
          steps {
                 sh '''#!/bin/bash
                 puppet resource file /tmp/clone ensure=absent force=true;
                 puppet resource file /tmp/clone ensure=directory;
	   cd /tmp/clone;
	   git clone https://github.com/bcsim-git/devops_repo.git;
                 targets='puppetclient1.localdomain';
                 locate_script='/tmp/clone/devops_repo/script_to_run';
                 bolt script run $locate_script -t $targets -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "QA container updated"
          }
          }
	  stage('Four') {
          steps {
                  echo 'QA Test - Begin ......'
		  echo 'QA Test - QA Report Generated'
          }
          }
          stage('Five') {
            steps {
                script {
          v1 = input ( 
                       message: 'Proceed to Production or Rollback',
                       parameters: [choice(name:'',choices: ['Proceed to Production', 'QA Rollback'])]
                       )
                       }
	           }
	    }          
          stage('Six') {
             steps {
                script {
                   if (v1 == 'Rollback') {
                   echo 'Rollback of QA container Completed'
                   } else if (v1 == 'Proceed to Production') {
                   sh '''#!/bin/bash
                   targets='puppetclient2.localdomain';
                   locate_script='/tmp/clone/devops_repo/script_to_run';
                   bolt script run $locate_script -t $targets -u clientadm -p user123 --no-host-key-check --run-as root;
                   '''
		   echo "Production container updated"
		   }	      
		 }
	  }
          }
      }
}

