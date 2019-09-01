# Watson Machine Learning to Planning Analytics
Want to amp up your Planning Analytics server with some machine learning?

<center><b>Well...now you can!</b></center>

This repo contains all the assets you need to get up and running with WML and PA. It goes through:
<center>
<table>
<td>
<li>Analysing and visualising sales data using Python and Seaborn</li>
<li>Training and tuning a Random Forest Regression model using Scikit learn</li>
<li>Deploying a ML model using Watson Machine Learning</li>
<li>Integrating the model into production with Planning Analytics</li>
<li>Visualising the results using Planning Analytics Workspace</li>
</td>
</table>
</center>

Want to try it out for yourself?

Well...in the words of the great Matthew McConaughey;

Alright, alright alight...let's kick it off.

## Step 1: Creating a Project and Loading Data into Watson Studio
First up, you'll need to create a new project in Watson Studio. Head on over to dataplatform.ibm.com and click <i>Create a project</i>. 
![Watson Studio Project](https://i.imgur.com/BK0NasE.png)

Name your project and hit <i>Create</i> in the bottom right hand corner. In this case, the project is named Watson Planning Analytics. Feel free to name it anything you'd like!
![enter image description here](https://i.imgur.com/GGThgmQ.png)

Once your project has been created. Click <i>Add to project</i> in the top right hand corner of Watson Studio to add a new data asset. Then click <i>Data</i>, this will allow you to load up the data files that will be used to train the model. 

<b>Side Note</b>: The actual training will be done using Python and Jupyter, these data assets can be imported easily once you've got them uploaded.
![enter image description here](https://i.imgur.com/M13LS2Q.png)

Once you've selected Data, a new drawer will open up on the right side of the screen. From here you can drop in your data assets. Go on ahead and drop in the four data assets that are within the Data folder in this repo. 
![enter image description here](https://i.imgur.com/Y9qgpLu.png)
<b>Here's a quick recap of the data that's available. </b>
<ul>
<li>IBMChem_Sales.csv - Main dataset that contains sales data by product, store and day.</li>
<li>IBMChem_Products - Metadata for products e.g. ID, Brand Affinity, Product Size, Category.</li>
<li>IBMChem_stores.csv - Metadata for stores e.g. ID, Address, Suburb, RegionAffluence.</li>
<li>Weather.csv - Temperature and Precipitation by Day and State</li>
</ul>
Now that's done, you're able to get to the good stuff...coding with Python!

## Step 2: Uploading and Setting Up the Jupyter Notebook
To get up and running faster, load up the Notebook in this repo <b>(IBMChem Notebook.ipynb)</b>. Inside of Watson Studio, select Add to Project and choose Notebook. 
![enter image description here](https://i.imgur.com/mF9AEBA.png)

Then select the From File option and choose the Notebook from your desktop. 
![enter image description here](https://i.imgur.com/D92ja47.png)

And, hit Create Notebook in the bottom right.
![enter image description here](https://i.imgur.com/86fgjYt.png)
Boom...notebook loaded!
![enter image description here](https://i.imgur.com/ZzSrzdj.png)

## Step 3: Updating API Credentials
The notebook has been setup so that you can drop your credentials in and get up and running. There are a few credentials that need to be updated in order to run through the code successfully. 

<b>3.1. Update IBM Cloud Object Storage API key</b>
This client allows you to access the data files that you uploaded in Step 1. Replace all fields that say \<YOUR IBM COS API KEY> with the api key. 
![enter image description here](https://i.imgur.com/fXNMQs4.png)
The easiest way to retrieve this key is by selecting the <i>Find and add data</i> button from the top right hand side of the page, selecting a dataset and choosing Add Credentials. The replace \<YOUR IBM COS API KEY> with the IBM_API_KEY_ID.  Take note of the BUCKET as well, you'll need this for step 3.2. 
![enter image description here](https://i.imgur.com/HEF70k6.png)

<b>3.2. Update the Bucket Name</b> 
Using the bucket name from step 3.1. replace all the fields that show \<YOUR BUCKET ID>. This allows you to point to the bucket that your data was deposited into from step 1. 
![enter image description here](https://i.imgur.com/FcgCBRR.png)

<b>3.3. Update WML Credentials</b>
The last thing to do is update the Watson Machine Learning credentials. The WML service allows you to deploy your model so that it can be used in production applications (in this case, it'll be used with Planning Analytics). 
![enter image description here](https://i.imgur.com/ARPqlNN.png)
To get these credentials, you'll first need to create a Watson Machine Learning service. Once this is done, follow these steps to get your credentials [Get WML Credentials](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-get-wml-credentials.html)

# Step 4: Run through the Notebook
Now run through the notebook. Each section will focus on a different topic within the CRISP-DM framework. The last few sections  will walk you through the steps to train and deploy your model to Watson Machine Learning.
![enter image description here](https://i.imgur.com/2mG9j5u.png)

One of the final steps is creating a test prediction against the deployed model. As part of this you'll access the scoring URL. <b>Run this cell and print out the scoring_url, you'll need this for the deployment to Planning Analytics</b>
![enter image description here](https://i.imgur.com/SIG6yg4.png)

# Step 5: Setup Planning Analytics
Included in the repo are assets for a TM1 Server and a Planning Analytics Workspace dashboard. Now we'll walk through how to spin up a the demo instance and hook up the plumbing.
<b>Side Note: This assumes you already have Planning Analytics and Workspace installed and configured.</b>

Extract the IBMChem TM1 Server folder to the server where your PA instance is installed. Navigate to the Configuration directory and make sure that you have an appropriate HTTPPortNumber set. This allows the PA REST API to be utilised alongside Watson Machine Learning.  
![enter image description here](https://i.imgur.com/UY9NcXc.png)

Then create a new server and start it up using Cognos Configuration.
![enter image description here](https://i.imgur.com/iCCDW0Y.png)

# Step 6: Update Scoring Script
Last but not least, update scoringscript.py (Located in IBMChem/Utilities/PAPredict) for your tm1 credentials. 
![enter image description here](https://i.imgur.com/kGt2Xzb.png)

And...update creds with your WML API Key, Instance ID and Scoring URL. 
![enter image description here](https://i.imgur.com/Zwa288a.png)

# Step 7: Upload PAW Dashboard and Run Prediction
Unzip and import the Planning Analytics Workspace Dashboard (IBMChem PAW Dashboard.gz). First select Import and choose the file.
![enter image description here](https://i.imgur.com/EE0qf9h.png)

The select the snapshot and hit <i>Migrate</i>. 
![enter image description here](https://i.imgur.com/Iv2fXoy.png)

You can then open the Dashboard and begin using the deployed model. Select Start to kick off the journey.
![enter image description here](https://i.imgur.com/iek58Zi.png)

The first dashboard allows you to review the sales data by Subcategory and State. Selecting each of the filters will allow you to review different slices of data. Once Done hit the Next button.
![enter image description here](https://i.imgur.com/4CSwE6Y.png)

This will take you to the prediction dashboard. The green line represents the prediction from Watson Machine Learning. Hit <i>Clear Prediction</i> to start again. Then select <i>Run Prediction</i> to generate a new prediction. Once the prediction is complete the results of the forecast will appear in the dashboard. Once done, select <i>Next</i>.
![enter image description here](https://i.imgur.com/khHn3HZ.png)

Last but not least you can review the results of the prediction using different slices of data. 
![enter image description here](https://i.imgur.com/vYyfPjC.png)

And thats it...an end-to-end model for using and deploying Watson Machine Learning and Planning Analytics! 

Happy Coding!
Nick