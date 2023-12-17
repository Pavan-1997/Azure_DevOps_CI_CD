# Azure DevOps CI/CD

1. Go to the dev.azure.com

    Pipelines -> Releases
    
    Release Pipeline doesnot provide YAML starter pipeline. Hence mostly the CI/CD is implemented using the Build Pipline
    
    Click on Azure App Service Deployment
    
    Give a Stage name - Test Deployment
    
    Now go to Artifacts -> Click on Add an artifact -> Select Build -> Project - Netflix -> Source - Netflix-Clone -> Default version - Latest -> Click on Add


2. Click on the Continuous deployment trigger -> Select Enable -> Click on Add -> Type - Include | Build branch - main


3. Click on the Pre-deployment conditions -> Select trigger - After release  -> Select Enable for Artifacts filters -> Click on Add -> Type - Include | Build branch - main

   Click on Enable for Gates -> Click on + Add -> Select Query Work Items


4. Now go to Boards -> Work items -> Click on New Work Item -> Select Task -> Name - Qaulity gate check ->  State - Doing  -> Click on Save 

    Now go to Queries on the left -> Click on + New queries -> Feild - Priority | Operator - = | Value = 1 -> Click on + New queries -> Feild - State | Operator - <> | Value = Done -> Remove all other filters -> Click on Save query -> Name - P1_Tasks | New folder - Shared Queries -> Click on OK
    
    Now go back to the Boards -> Work items -> Select the created Work Item -> Change the Priority - 1 -> Click on Save
    
    Now go back to the Boards -> Queries again -> Click on P1_Tasks -> Shows one item -> Click on the Charts section -> Group by - Work Item Type -> Click on Save chart
    
    Now again go back to the Boards -> Queries again -> Click on P1_Tasks -> Click on the More Actions -> Click on Security -> Search for Netflix Clone Build Service -> Read - Allow 
    

5. Now go back to the Step 3. -> In the Query Work Items -> Shared Queries -> Expand now you should see P1_Tasks (If not showing refresh the page) -> Upper threshold - 2 


6.  Now click on stage job,task -> Select your Azure subscription -> Click on Authorize which creates a service connection -> App type - Web App on Linux -> Select the App service name that was created earlier

    Now click on Run on Agent -> Agent Pool - Self hosted 
    
    Click on Deploy Azure App Service -> Select Deploy to Slot or App Service Enviroment -> Resource group <> -> Slot  


7. Now go to the Azure portal -> Click on the Web App service -> Click on Deployment slots on the left -> Click on the Add a slot -> Name - staging -> Clone settings from - spavanraj97 -> Click on Add


8. Now go back to the end of Step6. -> Slot -> Now you will see staging select it -> Package or folder - $(System.DefaultWorkingDirectory)/_Netflix-Clone/drop -> Runtime Stack - 1.0 (STATICSITE|1.0) -> Click on Save -> Give name to this pipeline as NF_Clone_CD


9. Go back to the pipeline created in the previous step -> Click on Edit -> Click on Clone the last stage -> Name - Prod Deployment

    Click on Pre-deployment conditions -> Click on Enable for Pre-deployment approvals -> Approvers - Self 
    
    Click on job,tasks -> Click on Deploy Azure App Service -> Uncheck Deploy to Slot or App Service Enviroment -> Click on Save


10. Now trigger the Build Pipeline 


11. Now the Release pipeline will be triggered -> Will fail in Pre-deployment gates because we have a blocking work item as Doing 

So go back to the Boards -> Work Items -> Select the item -> Change the State from Doing to Done -> Click on Save 

Now the Pre-deployment gate will succeed in next 5 minutes 


12. Now access the Web App Service URL prod which should show you the application 


13. Now go to the Azure portal -> Click on the Web App service -> Click on Deployment slots on the left -> You will see the 100% traffic is directed to the prod URL -> Click on Swap on the top -> Source - staging | Target - prod -> Click on Swap
