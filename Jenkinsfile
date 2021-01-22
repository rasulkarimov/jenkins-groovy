job("Job1_gitpull") {
description ("Pulls the code from GitHub")
  scm{
    github('rasulkarimov/jenkins-groovy','main')
  }
  triggers {
        scm('* * * * *')
    }
  steps {
    shell('sudo rm -rf /root/DevOpsAL/*  && sudo cp -rvf * /root/DevOpsAL/')
  }
}

job("Job2_deploy") {
description ("Deploy the code on  Kubernetes")
  triggers {
    upstream('Job1_gitpull','SUCCESS')
    }
    steps {
      shell('''sudo cd /root/DevOpsAL
sudo ls
if sudo kubectl get all | grep webdeploy
then
sudo kubectl delete all -l app=webdeploy
sudo kubectl delete pvc -l app=webdeploy
sudo kubectl create -f /root/DevOpsAL/webdeploy.yml
sleep 10
sudo kubectl get all
else
sudo kubectl create -f /root/DevOpsAL/webdeploy.yml
sleep 10
sudo kubectl get all
fi
sudo kubectl cp /root/DevOpsAL/hello.html $(sudo kubectl get pod|grep webdeploy|awk '{print$1}'):/var/www/html/index.html''')
    }
}

job("Job3_testing") {
description ("Test the code")
  triggers {
    upstream('Job2_deploy','SUCCESS')
    }
    steps {
      shell('''export status=$(curl -s -o /dev/null -w "%{http_code}" http://$(sudo kubectl get nodes -o jsonpath='{ ..status.addresses[0].address }'):30050/index.html)
if [ $status -eq 200 ]
then
exit 0
else
exit 1
fi''')
    }
  publishers {
    extendedEmail {
      recipientList('rasul.karimov@gmail.com')
      defaultSubject('Failed build')
      defaultContent('The build was failed')
      contentType('text/html')
      triggers {
        failure{
      attachBuildLog(true)
        subject('Failed Build')
        content('The build was failed')
        sendTo {
          developers()
          }
        }
      }
    }
  }
}

buildPipelineView('CI-CD_groovy') {
  title('CI-CD_groovy')
  displayedBuilds(3)
  selectedJob('Job1_gitpull')
  showPipelineParameters(true)
  refreshFrequency(2)
}
