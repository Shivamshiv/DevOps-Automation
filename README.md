# DevOps Automation
*This project is based on Continuous Integration and Continuous Deployment which automates the development and deployment operations of
web server using Git, Jenkins and Docker. It will launch the updated content within a single time-frame as the developer commit the
changes.*

### Setting up the developer environment
> - First I created a local git repository and initialize it.
```
mkdir devopsal4/
git init
```
> - link it to the remote GitHub repository
```
git remote add origin https://github.com/Shivamshiv/devopsal4.git
```
> - Create a file in the local repository
```
cat > linux.html
```
> - Add and commit this file in local repo
```
git add linux.html
git commit linux.html -m "first commit"
```
> - Push the file to the remote GitHub repo
```
git push -u origin master
```
> - Reference for the above command.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(151).png)

### Another developer working on branch
> - To make branch in the repository
```
git branch dev1
git checkout dev1
```
> - Change the file and Commit it. After that push it to the remote GitHub repo
```
cat >> linux.html
git commit linux.html -m "Commited by developer"
git push -u origin dev1
```
> - Reference for the above command.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(152).png)
> - To automate the push operation to remote GitHub repo create hook
```
#!/bin/bash
git push
```
> - Reference for the above command.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(159).png)

### To merge the work of different developer

> - To check log of the file and merge the dev1 branch to the master branch
```
git checkout master
git log
git log --oneline
git merge dev1
```
> - Reference for the above command.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(154).png)

### Creating docker container for development and testing
> - First check which port are in use and then use the port accordingly for listening
```
netstat -tnlp
docker run -dit -v /root/devopsal4:/usr/local/apache2/htdocs/ -p 8082:80 --name devlop_env httpd
docker run -dit -v /root/devopsal4:/usr/local/apache2/htdocs/ -p 8083:80 --name testing_env httpd
```
> - Reference for the above command.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(155).png)

### Create developer job in Jenkins
> - To automate the building of developer container via job chaining as soon as job copy is successful .
```
if sudo docker ps | grep devlop_env
then 
echo "Already running"
else
sudo docker run -dit -v /root/devopsal4:/usr/local/apache2/htdocs/ -p 8082:80 --name devlop_env httpd
fi
```
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(156).png)

### Create testing job in Jenkins
> - To automate the building of testing container via job chaining as soon as job copy is successful.
```
if sudo docker ps | grep testing_env
then 
echo "Already running"
else
sudo docker run -dit -v /root/devopsal4:/usr/local/apache2/htdocs/ -p 8083:80 --name testing_env httpd
fi
```
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(158).png)

### Quality and assurance job always trigger the updated developer job 
> - First link the quality and assurance job as a down_stream project to the upstream_project, developer job
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(157).png)

### To automate the website runnng from different environment attach hook
> - If developer will update the file in the master branch then Jenkins will fetch it from the developer_job and deploy
on the develop_env and if developer will update the file in the dev1 branch then Jenkins will fetch it from the testing_job
and deploy on the testing_env
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(161).png)

### Detail picturization of how automation of integration and deployment is taking place using Git, Jenkins and Docker.
![Image](https://github.com/Shivamshiv/devopsal4/blob/master/Screenshot%20(160).png)

*In future the updated job in dev1 branch which is deploying in testing_env will be merge to the master branch if the updation
is working fine.*

