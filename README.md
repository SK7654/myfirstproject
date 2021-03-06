![1](https://user-images.githubusercontent.com/64473684/84465195-022f1a00-ac94-11ea-87eb-d5d331c7afa2.jpg)
# Objective :

Setup up an infrastructure such as there are three teams/environemnts :

1. Production
2. Testing
3. Quality Assurance

Production Team will deploy the code first, now there is some work going on in the developement team which is to be deployed on testing environment which will be sent to the production only when the QA team approves it.

## Requirements :

1. Docker
   - Image: httpd
2. Jenkins
   - Github Plugin
3. System with 2gb ram (Recommended)
4. Git and Hooks
5. Github and Github Webhooks
6. ngrok

# My Approach

1. Made 3 Jobs for produnction, testing and Quality Assurance

2. Make Environments for production and testing. I am using docker here, I have created a volume(persistant storage) for production and testing and mounted them to their respective containers

## Process :

### Operations Side:

1. My job 1 and job2 will get their code from the github (from their respective branches => master:production, dev:testing)
2. Both the containers have diferent web addresses so the quality team can monitor both
3. Quality team continously checks for changes in testing environemnt, and decides wether to approves it or not
4. On approval QA team manually builds the job3
5. Since job three is the upstream job of job2 and job1 , so on succesfull approval foloowong things will happen:
   1. Dev and Master branch will be merged
   2. Code will be pushed to the github
   3. job 1 and job2 will be build

- ngrok will be running in the system to provide public accessable addres so that github webhook will work and production site can be accessible to the public

### Developer site :

1. Created a hook for post-commit and post-merge so the developer need not to be manually push the code each time he commits.
![1](https://user-images.githubusercontent.com/64473684/84466652-7d45ff80-ac97-11ea-89e0-1d1bb16e612d.PNG)
![1452](https://user-images.githubusercontent.com/64473684/84880680-77428b00-b0aa-11ea-99ad-8acfb9d32e6b.PNG)
2. Added The webhooks to the Jenkins Github API :=> ip/github-webhook/
![webhooks](https://user-images.githubusercontent.com/64473684/84473373-94d8b480-aca6-11ea-82bf-ecb0e698d38c.jpg)

## Screenshots :

## 1. Initially:

1. Production:

![hello 81](https://user-images.githubusercontent.com/64473684/84865595-20cb5180-b096-11ea-9abb-93745d1b710f.PNG)


2. Testing:

![6](https://user-images.githubusercontent.com/64473684/84867190-6721b000-b098-11ea-8ef2-54c6b0352594.PNG)

## 2. Changed made but not approved:

1. Production:

![hello 81](https://user-images.githubusercontent.com/64473684/84865595-20cb5180-b096-11ea-9abb-93745d1b710f.PNG)

2. Testing:

![6](https://user-images.githubusercontent.com/64473684/84867190-6721b000-b098-11ea-8ef2-54c6b0352594.PNG)

3. QA rejected

## Changed and approved:

1. Testing :

![82 COLOR](https://user-images.githubusercontent.com/64473684/84867721-43ab3500-b099-11ea-9c4c-1bcf480c36f1.PNG)


2. QA Approved

![501](https://user-images.githubusercontent.com/64473684/84882166-7874b780-b0ac-11ea-909d-0ebf1ecb2395.PNG)

![qa1](https://user-images.githubusercontent.com/64473684/84494220-bb0e4c80-acc6-11ea-80ae-fa2054fb3bf7.jpg)

![101](https://user-images.githubusercontent.com/64473684/84877608-5c6e1780-b0a6-11ea-8ce9-28a497dd864d.PNG)


3. Production

![81 COLOR](https://user-images.githubusercontent.com/64473684/84867941-8bca5780-b099-11ea-826b-ad11a8221e49.PNG)



## Steps and Configurations Screenshots :

## 1. Production:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will    contact jenkins, then jenkins will pull the updated code, and move it to the created volume.

![1 3](https://user-images.githubusercontent.com/64473684/84872042-f6ca5d00-b09e-11ea-8165-508c10169e60.PNG)


* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8081.

![1 4](https://user-images.githubusercontent.com/64473684/84872258-4a3cab00-b09f-11ea-9029-d1f3a5e23ae5.PNG)


## 2. Testing:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will contact jenkins, then jenkins will pull the updated code, and move it to the created volume.

![1 9](https://user-images.githubusercontent.com/64473684/84872467-92f46400-b09f-11ea-85fa-00387166d9c8.PNG)


* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8082 QA team will check the testing environment at this port address.

![1 6](https://user-images.githubusercontent.com/64473684/84872706-da7af000-b09f-11ea-8df7-65fb9623062e.PNG)


## 2. QA Team :

* Provided the github repo link along with credentials as on approval there should be a merge between the master and dev branch, and also the code should be pushed to github.

![1 22](https://user-images.githubusercontent.com/64473684/84874209-e8317500-b0a1-11ea-9724-a739abac8d8a.PNG)



* Details is provided to the additional behaviour so that merging could be done.

![1c01](https://user-images.githubusercontent.com/64473684/84503924-b2bf0d00-acd8-11ea-94ad-2c9bd3d91e34.jpg)

* Post Build Action deatils provided, so that push after the merge can be done

![1c03](https://user-images.githubusercontent.com/64473684/84504145-0f222c80-acd9-11ea-8a52-e536dea5fd4f.jpg)

* After the push is being done. This job will call the job1 and job2 so that the code can be updated. (This part can be omitted as webhook will do it's job, but to be on safe side, I have put these projects as downstream projects)
![1c04](https://user-images.githubusercontent.com/64473684/84504304-54def500-acd9-11ea-8d76-9d456ea27cc8.jpg)














