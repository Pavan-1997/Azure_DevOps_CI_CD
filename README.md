# Azure DevOps CI/CD

![CICD](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/35d89fc5-88f0-414a-8656-54c395012c50)


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

![WebApp](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/a7d37d2b-e8af-4762-b888-62934df02a39)


8. Now go back to the end of Step6. -> Slot -> Now you will see staging select it -> Package or folder - $(System.DefaultWorkingDirectory)/_Netflix-Clone/drop -> Runtime Stack - 1.0 (STATICSITE|1.0) -> Click on Save -> Give name to this pipeline as NF_Clone_CD


9. Go back to the pipeline created in the previous step -> Click on Edit -> Click on Clone the last stage -> Name - Prod Deployment

    Click on Pre-deployment conditions -> Click on Enable for Pre-deployment approvals -> Approvers - Self 
    
    Click on job,tasks -> Click on Deploy Azure App Service -> Uncheck Deploy to Slot or App Service Enviroment -> Click on Save


10. Now trigger the Build Pipeline 

![Build](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/214b0394-504d-428d-8b79-f69348d5fe75)


11. Now the Release pipeline will be triggered -> Will fail in Pre-deployment gates because we have a blocking work item as Doing 

    So go back to the Boards -> Work Items -> Select the item -> Change the State from Doing to Done -> Click on Save 
    
    Now the Pre-deployment gate will succeed in next 5 minutes 

    For Prod Deployment you will be requiring approval - self, you will also be getting an email as below 

![Approve-Done](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/b37abb9f-fb05-46d5-986a-522a2de2622e)

![Email](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/fdf6e973-f414-45ec-a2a2-be4422592a37)

![Release3](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/0a195a53-61c3-49ef-9144-42b12a1cf72a)

![Gate-Check-Prod](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/72d6ad62-dfa7-4b93-ab13-676363a36d70)

![Release1](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/8c9823d5-3eda-4a1a-9e15-e5eb3d08a289)

![Release](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/471fb160-bb34-4e5d-8613-c753b4c204b7)

![Release4](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/b948c22a-dda6-4a33-97a0-d514fdb9425d)



13. Now access the Web App Service URL prod which should show you the application 

![Prodcution](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/ce7980f5-3d79-4c1f-a68e-86b8296b30d3)


13. Now go to the Azure portal -> Click on the Web App service -> Click on Deployment slots on the left -> You will see the 100% traffic is directed to the prod URL -> Click on Swap on the top -> Source - staging | Target - prod -> Click on Swap

![Deployment-slots](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/9966aa4f-2204-4d0a-bea6-975adb388505)

![SWAP](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/4ed524ad-f6a5-44e0-b357-d88b02243864)

![Staging](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/9b5fbd28-552c-4cf6-93d0-118e8f0c5aa7)

---

### Below is the self hosted Agent I have used as Ubuntu Server running on EC2 AWS:

![AgentPool](https://github.com/Pavan-1997/Azure_DevOps_CI_CD/assets/32020205/55980290-5376-4c2a-8cb2-95c561a01711)
