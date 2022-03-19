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
                 bolt command run '
                 puppet resource file '/root/pull_devops_repo' ensure=absent force=true;
                 puppet resource file '/root/pull_devops_repo' ensure=directory;
                 puppet apply /root/pull_devops_repo/index_write;
                 cp -p /root/pull_devops_repo/index.html /var/www/html' 
                 -t puppetclient1 -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "Development container updated"
          }
          }
          stage('Four') {
          steps {
            input('Do you want to update to Production container: Proceed to Production')
                
          }
          }
          stage('Five') {
          when {
                not {
                    branch "Production NOT updated"
                }
          }
          steps {
                 sh '''#!/bin/bash
                 bolt script run '/root/pull_devops_repo/index_write' -t puppetclient1 -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "Prodcution container updated"
          }
          }



      }
}
