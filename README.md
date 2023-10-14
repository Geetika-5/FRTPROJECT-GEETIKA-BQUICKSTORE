# FRTPROJECT-GEETIKA-BQUICKSTORE
Fashion Supply Chain Optimization: Analyzing Order History for Holiday  Inventory Management for an online store. 

https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4914658340971203/2559883118436597/8373299318609809/latest.html
(DASHBOARD LINK)

Problem Statement :
Bquick Shopping, an online fashion store, faces the challenge of efficiently managing their supply chain to meet the increasing demand during the upcoming holiday season. They need a comprehensive solution to analyze their order history and ensure they have sufficient inventory. 
This project aims to develop a supply chain dashboard that utilizes data from BSinventory.json and BSorders.json files stored in AWS S3. The data will be processed using Azure Data Factory pipelines, validated, and moved to the appropriate locations. Finally, the data will be transformed, validated using Azure Functions, Databricks and integrated into an Azure Data Lake Storage to support inventory monitoring and optimization. A comprehensive dashboard will be developed using Databricks for in-depth analysis, enabling better decision-making regarding inventory management.

Services/Tools Used: Azure Data Factory, Azure Functions, Azure Data Lake Storage Gen2, Azure Key Vault, App Registration , AWS S3, Databricks  

Problem Description :
In the rapidly evolving world of e-commerce, meeting customer demands, especially during peak seasons like the holidays, is crucial for business success. The "Fashion Supply Chain Optimization" project is designed to streamline and enhance the supply chain for an online fashion store- Bquick Shopping Online Store, allowing the organization to efficiently manage inventory during the upcoming holiday season.
The process begins with gathering critical data from various sources, including BSinventory.json, BSorders.json hosted on AWS S3. Azure Data Factory (ADF) pipelines are employed to facilitate the seamless movement of these files to Azure Data Lake Storage (ADLS). Once the data is in ADLS, validation is performed using Azure Functions, allowing for the identification of any discrepancies or issues. Files that meet the validation criteria are moved to the Staging folder, while those with errors are routed to the Rejected folder.

Next, the data in the Staging area is processed and integrated into an ADLS Gen2, providing a centralized repository for further analysis. Utilizing Azure Databricks, a comprehensive and insightful dashboard is created, allowing the supply team to delve into the order history and inventory details.
Finally, to make this valuable dashboard accessible and user-friendly, Databricks tools are used. This project empowers the online fashion store to optimize its supply chain, enhance customer satisfaction, and maximize profitability during the high-demand holiday season.
Architecture Flow :-
The architecture flow for the "Fashion Supply Chain Optimization: Analyzing Order History for Holiday Inventory Management" project involves several steps to seamlessly manage data from source to visualization. Here's a high-level architecture flow for this project:
1.	Data Ingestion and Extraction:
•	Data files (BSinventory.json, BSorders.json, and BSupdateorders.json) are stored in AWS S3.
•	Azure Data Factory (ADF) is configured to periodically fetch the data from AWS S3 and move it to Azure Data Lake Storage (ADLS).
2.	Data Validation and Transformation:
•	Azure Functions are triggered to validate and transform the ingested data.
•	Valid data is moved to the Staging folder in ADLS, and invalid data is sent to the Rejected folder.
3.	Data Integration and Storage:
•	Data in the Staging area is processed and integrated into an Azure SQL Database for centralized storage.
4.	Data Analysis and Dashboard Creation:
•	Azure Databricks is used to analyze the integrated data in Azure SQL Database.
•	Databricks performs advanced analytics and transforms the data into a suitable format for visualization.
5.	Dashboard Visualization:
•	A dashboard is created using visualization tools in Databricks.
•	The dashboard provides insights into order history, inventory levels, product popularity, and other relevant metrics.
6.	Deployment and Visualization:
•	The dashboard is published on Databricks to make it accessible over the web.
This architecture ensures a streamlined flow of data from various sources, effective data processing, and meaningful visualization for data-driven decision-making in the fashion supply chain.


ARCHITECTURE DIAGRAM
![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/9af336c4-100f-49be-9d47-b7a294cde88d)

Project Phases:
•	Data Ingestion and Transfer:
•	AWS S3 BUCKET DATA FILES-JSON FORMAT
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/b19c1a24-7354-4b40-9ec8-8c6a319bcd0a)

•	Set Up Data Ingestion: Create data ingestion pipelines using Azure Data Factory to transfer data from AWS to Azure securely.I have stored the access key and secret access key from AWS S3, in Azure Key Vault, as best practices.Set up an Azure Data Factory pipeline using copy activity, to ingest data from the third-party IoT devices hosted on AWS. copy activity is used to move data from AWS S3 buckets to Azure Storage ADLS Gen2 input/landing folder.
•	RESOURCE GROUP AND RESOURCES IN AZURE PORTAL:-
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/3a62ac41-df89-4e89-bb06-f80af2119492)


AZURE KEY VAULT
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/250c4aaa-ed3d-41d4-b9a7-5f415113d44a)

•	Data Landing Zone:
•	Create a landing folder in Azure Storage to temporarily store the incoming JSON data files. You can organize this storage account with a container specifically for landing data.
•	input/landing
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/ab756354-7927-44cb-b728-d4ee7d49c655)

•	Azure Function for Validation:
•	Develop an Azure Function that uses a storage-based trigger (Blob Trigger-BlobTrigger1) to monitor the landing folder for incoming JSON files.
•	When a new file arrives, the Azure Function is triggered to validate the JSON format and content. If validation fails, move the file to the "rejectedFolder" folder; otherwise, move it to the "stagingFolder" folder.
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/25ff362b-f296-423b-af3e-f242aab33098)

•	Data Validation and Movement:
•	Configure the Azure Function to perform JSON validation using built-in JSON parsing libraries and validation logic to check the correctness of JSON files.
•	Use Azure Function bindings to move files between folders based on the validation result. If validation passes, move the file to the "stagingFolder" folder; if it fails, move it to the "rejectedFolder" folder.
module.exports = async function (context, myBlob) {
    context.log("JavaScript blob trigger function processed blob \n Blob:");
    context.log("********Azure Function Started********");
    var result =true;
    try{
        context.log(myBlob.toString());
        JSON.parse(myBlob.toString().trim().replace('\n', ' '));
    }catch(exception){
        context.log(exception);
        result =false;
    }
    if(result){
        context.bindings.stagingFolder = myBlob.toString();
        context.log("********File Copied to Staging Folder Successfully********");
    } else{
        context.bindings.rejectedFolder = myBlob.toString();
        context.log("********Inavlid JSON File Copied to Rejected Folder Successfully********");
    }
context.log("*******Azure Function Ended Successfully*******");  
};
•	Staging Data in Azure SQL DB:
•	Create an Azure SQL Database to serve as the staging area for your data. Design the database schema to accommodate the JSON data structure.
•	Develop another ADF Pipeline that triggers when files are moved to the "stagingFolder" folder. This function reads the JSON data, transforms it if needed
PIPELINE RUNS:-
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/0a73daaf-9306-4f88-b04e-af1337a22d48)

 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/8bbbbc6f-a9ed-43df-a8a3-1fc8dcf7eea7)


 TRIGGER RUNS ➖
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/2e9378da-13ed-4e56-8a40-cf1eff3731d1)

SOURCE:-
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/3f3a156e-49eb-4fed-bfa1-4d8c836c0188)

SINK
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/463e9a56-c421-4eec-b696-c25f7e32241b)

ADLS CONTAINER AFTER PIPELINE RUN:-
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/b6fab32c-6048-4bcd-8e76-44e2e37460fd)

 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/b3845fef-b913-44b8-9e2d-b74dd5a57834)

 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/f48d605e-28db-4482-9737-5aa6776e5735)


PIPELINE 1 SUCCESSFUL:-DATABRICKS MOUNTING
 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/f32cce2a-db3d-4901-ba86-d5fddeac9294)

 ![image](https://github.com/Geetika-5/FRTPROJECT-GEETIKA-BQUICKSTORE/assets/125957690/c97695db-0706-457b-90da-452c0e1c466c)



