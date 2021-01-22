# Ci/Cd pipline on grovy script
##Seed Job
* Create Seed Job and put Jenkinsfile, all required jobs will be created from groovy script:
![image](https://user-images.githubusercontent.com/53195216/105552923-b78b7b00-5d15-11eb-8991-c083f23f6d78.png)
---
![image](https://user-images.githubusercontent.com/53195216/105553027-e0137500-5d15-11eb-996b-f5e67e1182a0.png)

* Click Build Job, then Job1_pull, Job2_deploy, Job3_testing will be created. Code automatically downloaded from git, webserver deploed and tested.
![image](https://user-images.githubusercontent.com/53195216/105553933-6b413a80-5d17-11eb-9273-4cbf749c972a.png)
* Webpage available on port 30050, after changing code on the repo, the webserver automatically will be redeployed.
![image](https://user-images.githubusercontent.com/53195216/105553983-81e79180-5d17-11eb-8cad-199c4ab02664.png)
