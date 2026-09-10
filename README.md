# CI/CD Assignment 1 – Jenkins Git Operations + Ninja File Pipeline

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#cicd-assignment-1--jenkins-git-operations--ninja-file-pipeline)
Submitted by Devashish Sathawane
Two Jenkins pipelines with Slack and Email notifications on every build.
**Part 1:** A parameterized job to perform Git branch operations (create, list, merge, rebase, delete). **Part 2:** A two-job chain - first job creates a file with content, second job (auto-triggered) publishes it via a web server.

## Setup

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#setup)

### Install Jenkins

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#install-jenkins)

```
jenkins --version
```

svg
<img width="1440" height="900" alt="Screenshot 2026-09-09 at 12 22 32 AM" src="https://github.com/user-attachments/assets/17e15ef5-8f45-4513-b4fd-463bfd6d29f5" />


```
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

svg
<img width="1440" height="900" alt="Screenshot 2026-09-09 at 12 23 28 AM" src="https://github.com/user-attachments/assets/341c4b3b-ef53-433c-a648-b4d0f64cc280" />


### Create the assignment repo

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#create-the-assignment-repo)
Created `jenkins-assignment-repo` on GitHub.

[image](https://private-user-images.githubusercontent.com/181391629/645927509-5fbb10fc-782d-4f7b-9e1e-9023d43b316b.png)

### Add GitHub credential to Jenkins

<img width="1440" height="900" alt="Screenshot 2026-09-09 at 12 33 19 AM" src="https://github.com/user-attachments/assets/a46fde55-fa20-4457-9e2f-5786d1679563" />


```
Manage Jenkins → Credentials → System → Global → Add Credentials
```

[image](https://private-user-images.githubusercontent.com/181391629/645927606-51ddd9e5-b10c-45b3-9f5b-20d402a234cd.png)

## Slack Integration

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#slack-integration)

### Create a Slack App

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#create-a-slack-app)

```
api.slack.com/apps → Create an App
```

<img width="1440" height="900" alt="Screenshot 2026-09-09 at 11 39 28 AM" src="https://github.com/user-attachments/assets/69d6c9fc-700b-4c77-93fc-cda8820889f5" />


### Get the Bot OAuth Token

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#get-the-bot-oauth-token)

```
xoxb-************-************-********************
```

[image](https://private-user-images.githubusercontent.com/181391629/645927873-3e977626-8a9d-4bab-9129-c6910426d9e6.png)

### Add the Jenkins bot to Slack

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#add-the-jenkins-bot-to-slack)

Invited the "Jenkins CI" app to the `#jenkins-notifications` channel.

<img width="1440" height="900" alt="Screenshot 2026-09-01 at 8 55 04 PM" src="https://github.com/user-attachments/assets/9e593929-fe36-4729-afad-b8f52d25e302" />


### Add Slack credential in Jenkins

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#add-slack-credential-in-jenkins)

```
Manage Jenkins → Credentials → Add Credentials → Secret text (Slack token)
```

[image](https://private-user-images.githubusercontent.com/181391629/645928157-64c9f146-7982-40bc-aed8-ddb7561fdd55.png)

### Configure Slack notifications and test connection

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#configure-slack-notifications-and-test-connection)

```
Manage Jenkins → System → Slack → Workspace, credential, default channel #jenkins-notifications
```

[image](https://private-user-images.githubusercontent.com/181391629/645928303-35f3123d-3630-4664-84ac-cc6d258afca3.png)

Test message received in Slack:

[image](https://private-user-images.githubusercontent.com/181391629/645928483-7347a1d6-b83b-4021-9711-450a6e2ecf73.png)

## Email Integration

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#email-integration)

### Add Gmail credential (App Password)

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#add-gmail-credential-app-password)

```
Google App Password used: piqqgmmgrjaqaffg
```

[image](https://private-user-images.githubusercontent.com/181391629/645928686-c7f9aa17-55bc-4b44-9d35-189e62f8317e.png)

### Test email configuration

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#test-email-configuration)

```
Manage Jenkins → System → Extended E-mail Notification → Test configuration by sending test e-mail
```

Email was sent successfully.

<img width="1440" height="900" alt="Screenshot 2026-09-01 at 7 50 31 PM" src="https://github.com/user-attachments/assets/07537582-df06-441f-9aef-9acb24fe18b6" />


## Part 1: Git-Branch-Operations Job

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#part-1-git-branch-operations-job)

### Job configuration - Choice parameter for the 5 operations

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#job-configuration---choice-parameter-for-the-5-operations)

```
ACTION (Choice Parameter): CREATE_BRANCH, LIST_BRANCHES, MERGE_BRANCH, REBASE_BRANCH, DELETE_BRANCH
BRANCH_NAME (String Parameter)
TARGET_BRANCH (String Parameter)
```

<img width="1440" height="900" alt="Screenshot 2026-09-09 at 1 27 50 PM" src="https://github.com/user-attachments/assets/f730445b-2c78-4429-9bae-a87b2777cea1" />


### SCM configuration

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#scm-configuration)

Points to `jenkins-assignment-repo`, branch `*/main`, using the `github-creds` credential.

<img width="1440" height="900" alt="Screenshot 2026-09-09 at 1 38 25 PM" src="https://github.com/user-attachments/assets/5551c255-2d2d-47cb-81b1-e74ff5737cb1" />


### Build step (Execute shell) with a `case` block for each action, plus Email post-build action

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#build-step-execute-shell-with-a-case-block-for-each-action-plus-email-post-build-action)

```
case "$ACTION" in
  CREATE_BRANCH) ... ;;
  LIST_BRANCHES) ... ;;
  MERGE_BRANCH)  ... ;;
  REBASE_BRANCH) ... ;;
  DELETE_BRANCH) git push origin --delete "$BRANCH_NAME" ;;
  *) echo "Invalid action"; exit 1 ;;
esac
```

<img width="1440" height="900" alt="Screenshot 2026-09-09 at 1 45 21 PM" src="https://github.com/user-attachments/assets/f3e38e29-5be2-4124-92c9-e4ab6097318e" />


### Slack notification post-build action

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#slack-notification-post-build-action)

"Notify Every Failure" checked, so a Slack message goes out whenever a step fails.

<img width="1440" height="900" alt="Screenshot 2026-09-10 at 3 26 51 PM" src="https://github.com/user-attachments/assets/03414608-8046-493d-b666-3c9e06f63090" />


### Job overview

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#job-overview)

`Git-Branch-Operations` job with build history showing multiple successful and failed runs.

<img width="1440" height="900" alt="Screenshot 2026-09-10 at 8 53 30 PM" src="https://github.com/user-attachments/assets/6f7bfa4a-91a9-4b38-af7e-33eab7185090" />


### List all branches

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#list-all-branches)

```
ACTION = LIST_BRANCHES
```

Console shows local and remote branches.

<img width="1440" height="900" alt="Screenshot 2026-09-10 at 8 56 11 PM" src="https://github.com/user-attachments/assets/4b5c4327-027c-4aae-aad5-75629213798e" />


### Merge one branch into another

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#merge-one-branch-into-another)

```
ACTION = MERGE_BRANCH, BRANCH_NAME = Deva, TARGET_BRANCH = main
```

```
SUCCESS: Merged 'Deva' into 'main'.
```

[image](https://private-user-images.githubusercontent.com/181391629/645930122-20fcbb4e-5178-4e2a-b0cf-081b767f2ac3.png)

### Delete a branch that doesn't exist (to trigger the failure path)

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#delete-a-branch-that-doesnt-exist-to-trigger-the-failure-path)

```
ACTION = DELETE_BRANCH, BRANCH_NAME = Raj, TARGET_BRANCH = main
```

[image](https://private-user-images.githubusercontent.com/181391629/645930327-524d690e-1b27-481a-a1eb-b7fe8f65ec52.png)

### Slack failure notification received

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#slack-failure-notification-received)

```
Git-Branch-Operations - #16 Failure after 0.64 sec
```

[image](https://private-user-images.githubusercontent.com/181391629/645930537-57dfdfe2-cde0-45a5-a894-f9c5f9d7e496.png)

### Email failure notification received

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#email-failure-notification-received)

```
Git-Branch-Operations - Build # 16 - Failure!
```

[image](https://private-user-images.githubusercontent.com/181391629/645930698-3220ae77-0b28-4ddd-9d14-42d8bc1ca6bf.png)

## Part 2: Create-Ninja-File → Publish-Ninja-File

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#part-2-create-ninja-file--publish-ninja-file)

### Job 1: Create-Ninja-File

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#job-1-create-ninja-file)

Takes `Ninja_Name` as a string parameter, writes `"<Ninja Name> from DevOps Ninja"` to a file, and archives it as an artifact (`ninja_output.txt`). `Publish-Ninja-File` is configured as a downstream project so it triggers automatically after this job succeeds.

[image](https://private-user-images.githubusercontent.com/181391629/645931130-7ae191bb-f583-4f23-8719-ac1c7b7d2e27.png)

### Run: Build with Parameters → Ninja_Name = Arjun → Build

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#run-build-with-parameters--ninja_name--arjun--build)

Slack shows the chain of notifications - Git-Branch-Operations failures earlier, then `Publish-Ninja-File - #1 Success` firing automatically right after `Create-Ninja-File` completed.

[image](https://private-user-images.githubusercontent.com/181391629/645931808-1956a66d-fa49-45f2-893d-7702fc62619a.png)

### Email success notification for the downstream job

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#email-success-notification-for-the-downstream-job)

```
Publish-Ninja-File - Build # 1 - Successful!
```

[image](https://private-user-images.githubusercontent.com/181391629/645932027-4cb1274c-bb0b-406b-8fd6-8a13e4df646c.png)

### Verify the file is being served by the web server

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#verify-the-file-is-being-served-by-the-web-server)

```
http://54.87.2.175/ninja_output.txt
```

```
Arjun from DevOps Ninja
```

[image](https://private-user-images.githubusercontent.com/181391629/645932224-db8ce302-87df-4011-97d0-08ae80455f2f.png)

## Final Dashboard

[svg](https://github.com/OT-MyGurukulam/Jenkins_35#final-dashboard)

All three jobs (`Create-Ninja-File`, `Git-Branch-Operations`, `Publish-Ninja-File`) green and healthy.

[image](https://private-user-images.githubusercontent.com/181391629/645932458-c50059a1-ce34-42ea-b95f-30ebd32e8381.png)

