# Use Jenkins with GitHub on the docker

## Run Jenkins container

* https://registry.hub.docker.com/_/jenkins/
* Running container is so simple
`docker run -d --name my-jenkins -p 8888:8080 -v /home/core/jenkins:/var/jenkins_home jenkins` (In my case, I used CoreOS-beta-557.2.0-hvm instance in aws ec2)

    If you face an error like this

        error ---- mkdir: cannot create directory '/var/jenkins_home/init.groovy.d': Permission denied
        cp: cannot create regular file '/var/jenkins_home/init.groovy.d/tcp-slave-angent-port.groovy': No such file or directory

    You can solve with typing this code

        sudo chown 1000 /home/core/jenkins/

    https://github.com/cloudbees/jenkins-ci.org-docker

    "Ensure that /your/home is accessible by the jenkins user in container (jenkins user - uid 1000)."

* If your container running successfully, you can see your Jenkins page : http://YOUR_IP:8888/

## Install Jenkins GitHub plugin

* In your Jenkins page, Click **Manage Jenkins**
  - Click **Manage Plugin**
  - Move to **install available**
  - Search **github plugin**
  - After search, check **github plugin** and click **install without restart**

## Create the Project

* After install, Click **New Job**.
  - Write **name** of project, check **freestyle project** and click **OK**
* If you installed github plugin correctly, you can see **GitHub project**.
  - Write your github repository URL (use blue question mark)
* In **Manage source code**, check **git**.
  - Write your github repository URL (Specify the URL of this remote repository. This uses the same syntax as your git clone command.)
  - In Credential, click **Add** and add add credential(In my case, used Username and password)
* In **Build Triggers**, check **build when a change is pushed to Github**.
* You can also set **Build** and **Post-Build Actions** you want. And **save**

## Oauth token

* Move to your Github page -- https://github.com
  - In the top right corner of any page, click settings.
  - In the user settings sidebar, click **Applications**.
  - Click **Generate new token**.
  - Write your token description. check **admin:repo_hook** and click **Generate token**.
  - https://help.github.com/articles/creating-an-access-token-for-command-line-use
* In your Jenkins page, click **Manage Jenkins**.
  - Click **Configure System**, find github web hook.
  - Check **Let Jenkins auto-manage hook URLs**.
  - Check **Override hook url**.
  - Write **github credentials**(your github username) and **Oauth token**.
  - Click **Test credential** -- verified(fine). And **save**

## SSH public key

* When you register user, you have to write plublic key
  - https://help.github.com/articles/generating-ssh-keys/

## How to use

* Just commit and push in your repository
* Jenkins will automatically build(what you set).
