# What is DevOps
We have learned about many different teams that help to create a software product:
- Dev Team
    - These are the people who work on the product directly: they create the Repository, Service, and API layers of the application, they update the code base and add on new features, etc
- SRE Team
    - These are the people who monitor the application and associated software that is deployed with the application. These people do things like monitoring SLIs and alerting the proper people when SLIs indicate that SLOs either are not being met, or the SLO is at risk. SREs are also first on the scene when an incident occurs, and they are the ones who start/end a postmortem to try and avoid/mitigate such incidents in the future
- Ops Team
    - If the Dev Team works on the product, and the SRE Team monitors product, the Ops Team is responsible for handling the "pipeline" the product goes through to get from the dev source code to the production environment where the end user can make use of the application. They are also in charge of making sure the necessary resources for the app to work are present (RDS instance for the app to connect to, cluster for the app to be deployed in, etc). 

"DevOps" is a name given to the practice of combining development and ops practices into a single, streamlined process that is ideally as automated as possible. 

## DevOps Steps
There are 5 steps to the DevOps practice:
1. Source Code Control
    - This step involves storing your code and updating your code in a central location. Ideally this central location should also provide some means of version control (think Github). 
2. Building/Testing Automation
    - This step involves building you application and running any necessary tests to ensure your code works
3. Deploying to staging
    - This step involves sending the built/tested application to a staging environment
    - This "staging" environment is where you can perform final testing on the application
4. System Testing
    - This step involves the actual testing of the application
        - Acceptance testing
        - System testing
        - End to end testing
        - etc. 
    The key to recognize is that final testing that was unable to be accomplished in step 2 happens in this fourth step
5. Deployment of Build
    - This step involves taking the fully tested application and sending it to the production environment, thus allowing end users to interact with the application

# DevOps in Agile Methodology
In Agile methodologies (like Scrum) that make use of DevOps practices there are a few terms that are used to describe what levels of DevOps automation/practice is being used: 

- *Continuous Integration* (CI) # Step 1
    - This is the practice of consistently merging code into a central repository that helps to ensure everyone involved in the process is working the same up to date source code
    - This practice also involves code reviews before allowing new work to be merged into the main source code location (like a main branch of a git repository)
- *Continuous Delivery* # Steps 2-4
    - This is the practice of automating as much as is reasonable of steps 2-4 of the DevOps process
    - Continuous Delivery does not require that you automate EVERYTHING in the DevOps process, as this would be an unreasonable demand on many companies. The ideal is to automate as much of the DevOps process as is reasonable for your situation. This will depend on factors such as resource availability, team size, scope of your application, etc. 
    - Because of this, the term is somewhat nebulous, so if you are ever told your team practices Continuous Delivery you should ask follow-up questions to better understand the DevOps pipeline you are going to work with
- *Continuous Deployment*
    - This is the all encompassing DevOps practice: if you can automate your entire DevOps pipeline reasonably then you are practicing Continuous Deployment
    - When things don't go wrong, a Continuous Deployment practice allows for the entirety of the pipeline to be automated, and a good Continuous Deployment pipeline will have safeguards in place so taht if something does go wrong the pipeline can handle it and ensure that faulty code does not get sent to the production environment and exposed to end users.
