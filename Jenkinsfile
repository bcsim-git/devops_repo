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
            input('Do you want to update to Development container?')
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
                 targets=puppetclient1;
                 locate_script='/tmp/clone/devops_repo/script_to_run';
                 bolt script run $locate_script -t $targets -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "Development container updated"
          }
          }
          stage('Four') {
            steps {
                script {
          abc = input ( 
                       message: 'Proceed to Production or Rollback',
                       parameters: [choice(name:'',choices: ['Production', 'Rollback'])]
                       )
                       }
	           }
	    }          
          stage('Five') {
             steps {
                script {
                   if (abc == 'Rollback') {
                   echo ' Rollback'
                   } else if (abc == 'Production’) {
                   sh '''#!/bin/bash
                   targets=puppetclient2;
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

