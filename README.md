# Automated WebSite Deployment using Docker

![](images/Git-Docker-Jenkins.png)


# How it Works
This project demonstrates basic integration in the DevOps domain. When the developer commits code, it is automatically pushed to the respective GitHub Repository. Using GitHub WebHooks, Jenkins jobs are triggered automatically as soon as the code is pushed. I have also used the Build Pipeline concept for easy job monitoring.<br><br>
<b>Job-1</b> - Copies the code from GitHub to the Jenkins Workspace/folder.<br>
<b>Job-2</b> - Launches a Docker container with the specifications needed to deploy the website.<br>
<b>Job-3</b> - Tests the website to ensure it is working properly.<br>

All these jobs are automatically triggered as soon as the developer pushes the code to GitHub.

# How to Create this Project
The project may seem complex, but with a strong grasp of the concepts, it's manageable. Start by setting up Git, Jenkins, and Docker on your system. <br>
In Job-1's execute shell write<br>
  <code>sudo cp -rvf * /name_of_your_WorkspaceFolder</code> and then save it.<br><br>
In Job-2's execute shell write,<br>
  <code>if sudo docker container ls | grep production</code><br>
  <code>then</code><br>
  <code>sudo docker conatiner rm -f production</code><br>
  <code>else</code><br>
  <code>sudo docker dun -dit -p 8081:80 -v/name_of_your_WorkspaceFolder:/usr/local/apache2/htdocs --name production httpd</code><br>
  <code>fi</code> and then save it and go and create Job-3<br><br>
In Job-3's execute Shell write<br>
  <code>status=$(curl -o Your_SysIP:8081/)</code><br>
  <code>if[[ $status == 200 ]]</code><br>
  <code>then</code><br>
  <code>echo "Successfull"</code><br>
  <code>exit 0</code><br>
  <code>else</code><br>
  <code>echo "Error"</code><br>
  <code>exit 1</code><br>
  <code>fi</code> and then save it and come out of Job-3<br><br>
  
After Writing the codes, the next task will be to link all these using build Pipeline so that at one go all will Jobs run and and it will be easy to monitor. Before creating build ppipeline we need to set <b>build triggers</b> such that when Job1 will run, automatically it will triggered Job2 and Job3. <b>Job1 --> Job2 --> Job3</b><br>
Here is the image I have attached so that You can easly set the triggers. Please note that trigger is set as per the basis of UpStreams and DownStreams. In this I will setupp the trigger to Job2 such that, as soon as Job will successfully run, it will trigger to Job2 and similarly as Job2 will run successfully it will trigger to Job3 and hence it will create CI/CD.<br><br>
![](images/Trigger.png)<br>

Now as you have set the triggers, to each job, now it's time to create a pipeline so that we can easliy manage the whole CI/CD Pipeline. If you don't know how to set-up Build pipeline, then you just need to download the <b>Build Pipeline Plugin</b> from Plugin Manager in Jenkins and then I have uploaded the images for your convienent in image folder with names pipeline-1 and pipeline-2 for creating a pipeline for any project. If all the three Jobs will run succesfully then you will get green color in every Job as you can see in the below picture.<br>

![](images/Build%20Pipeline.png)<br>
<br>
