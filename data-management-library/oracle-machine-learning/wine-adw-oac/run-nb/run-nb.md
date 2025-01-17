# Import and Run the Machine Learning Notebook

## Introduction

Oracle Machine Learning notebook Apache Zeppelin comes with your Autonomous Database. Once data is loaded into the Autonomous Database, you can start using Oracle Machine Learning notebooks to investigate the data and run machine learning models with it. In this lab, you will log in as OMLUSER and then import and run the Oracle Machine Learning notebook.

Estimated Lab Time: 15 minutes

### About Product/Technology

With the classification techniques, the business problem defined here is a good model with
- Whether it is a good or bad bottle of wine
- Good wine having a score of greater than 90 points
- Bad wine having a score of less than 90 points

The data that we are using is not a standard structured data set. For example, we have wine reviews saying "Oh this wine has a very robust flavor!", "It smelt the aroma of cherries" and so on.

So we are using Oracle text mining to filter all the unstructured data stored in the database as a (character large object).

### Objectives

In this lab, you will:
* Upload the provided OML notebook
* Run the OML notebook
* Explore the notebook features

### Prerequisites

* Provisioned an ADB instance
* Created an OMLUSER account
* Uploaded data for OMLUSER


## **STEP 1**: Upload Notebook to Oracle Machine Learning

1.  From the hamburger menu, select **Autonomous Data Warehouse** and navigate to your Autonomous Database instance.

    ![](./images/choose-adb.png " ")

    ![](./images/choose-adb-adw.png " ")

2.  Select **Tools** on the Autonomous Database Details page.

    ![](./images/tools.png " ")

3.  Select **Open Oracle ML User Administration** under the tools menu.

    ![](./images/open-ml-admin.png " ")

4. Sign in as **Username - ADMIN** with the password you used when you created your Autonomous Database instance.

    ![](./images/ml-login.png  " ")

5.  Click the **Home Icon** in the top right corner.

    ![](./images/oml-home.png  " ")

6. Sign in as **Username - OMLUSER** and with the password you used when you created the OMLUSER account.

    ![](./images/oml-user-login.png  " ")

7.  We will be importing a **Picking Good Wines $30 Using Wine Reviews.json** ML notebook in this lab. Click [here]() to download the notebook. 

8. Click on the upper-left hamburger menu and select **Notebooks**.

    ![](./images/nb-menu.png  " ")

    ![](./images/notebooks.png  " ")

9. Click on **Import** and upload the notebook downloaded earlier.

    ![](./images/import.png " ")

10. Once the notebook is uploaded, click on the **Picking Good Wines < $30 Using Wine Reviews** notebook to view.

    ![](./images/import-2.png " ")

    ![](./images/notebook.png " ")

## **STEP 2**: Prepare and Run your Notebook

1. In the Picking Good Wines < $30 Using Wine Reviews notebook, click on the **gear** icon in the upper right. 

    ![](./images/binding.png " ")

2. We must set the interpreter binding if we're going to connect to the Autonomous Database instance and run queries. Ensure at least one of the bindings is selected, then click save.

    ![](./images/binding2.png " ")

3. Click on run button. 

    ![](./images/run.png " ")

4. In the pop up click on **OK** to run the notebook. If any paragraphs have an error, click the **run** button for them individually until they are finished.

    ![](./images/run-all-paragraphs.png " ")

## **STEP 3**: Explore the Notebook
Explore the data with the focus on points, price, province, region, Taster\_Name, taster\_Twitter_handle.

![](./images/explore.png " ")

1. Before converting the description into a clob object, first, alter the table to add a points_bin column to the table.

    ![](./images/add-points-bin-column.png " ")

2. Populate the points_bin column to do a classification to know whether a wine is good or bad i.e., as greater than 90 points and less than 90 points.

    ![](./images/populate.png " ")

3. Divide the table to segregate all of the wines to have points greater than 90 with a tag greater than 90 points and similarly less than 90 with the tag less than 90 points.

    Scroll right to the table to view the points_bin column with the tags .

    ![](./images/seggregate.png " ")

    ![](./images/populate-points.png " ")

4. Then divide the table into trainer and test data set, to run or build our model on training data and then test model on testing data.

    ![](./images/train-test-data.png " ")

5. We will be converting the description column from varchar2 to clob object by adding a new column, setting the previous description column to a new column and then dropping the old column. This is for use in Oracle text mining.

    ![](./images/description-to-clob.png " ")

6. Notice that the description is now clob data type.

    ![](./images/description-clob.png " ")

7. Change the Description attribute from varchar2 to clob for training data and display the metadata.

    ![](./images/train-data.png " ")

    ![](./images/train-data1.png " ")

8. Change the Description attribute from varchar2 to clob for test data and display the metadata.

    ![](./images/test-data.png " ")

    ![](./images/test-data1.png " ")

## **STEP 4**: Data Understanding

Now, let us understand how the data is distributed in our table to see how many reviews or how many wines are in our data set, and which wines are greater than 90 points and less than 90 points.

1. In the wine points ratings distribution, you can see that the lighter shade of blue are less than 90 points and darker shade of the blue are greater than 90 points.

    ![](./images/distribution.png " ")

2.  If you hover over one of the bars in the graph it gives you the actual point value and the total number of records that you have for that value.

    In this example, when we hover over the 89 points bar we have 96 records.

    ![](./images/hover.png " ")

    Note that Oracle Machine Learning notebooks by default uses Zepplin graph to show a simple visualization that takes the top 1000 records. If the highly computational values are at the bottom of the database Oracle Machine Learning notebook, the values may vary when compared to the actual results.

3. Explore the data based on top 10 countries and display the count of wines. Note that the U.S. is leading, followed by France and so on.

    ![](./images/country.png " ")

4. Apache Zepplin notebook allows six different types of graphs - table, bar chart, pie chart, area chart, line chart and scatter chart to visualize.

    Here we are comparing wines from Australia, California and Spain countries by regions using pie chart.

    ![](./images/pie-chart.png " ")

5.  We are doing this as a classification problem but the model we created has a classification column which is greater than 90 points and less than 90 points, which we don't need. Create a new table WineReviews130KTarget without the points attribute from WineReviews130K.

    ![](./images/drop-points.png " ")

6. View the WineReviews130KTarget table.

    ![](./images/target-table.png " ")

## **STEP 5**: Unstructured Data Preparation using Oracle Text

In order to use the reviews in the description column of the WineReviews130KTarget table in our machine model, we use Oracle text to do mining of unstructured data. You can apply data mining techniques to text terms which are also called text features or tokens. These text terms can be a group of words or words that have been extracted from text documents and their assigned numeric weights.

1. Drop the existing Lexer preference for repeatability

    ![](./images/drop-lexer.png " ")

2. Create a new Lexer preference for text mining by specifying the name of the preference you want to create, the type of Lexer preference you want from the types Oracle has and the set of attributes for your preference.

    In this example, `mylex` is the name of the preference, `BASIC_LEXER` is the type of Lexer preference and setting attributes as `index_themes`, `index_text`.

    ![](./images/create-lexer.png " ")

3. Create another preference for the basic word list and set the attributes for text mining.

    In this example, `mywordlist` is the name of the `BASIC_WORDLIST` preference, with attributes set for language as english, score as 1, number of results as 5000.

    ![](./images/list-lexer.png " ")

4. Drop an existing text policy for repeatability and create a new text policy for description for the Lexer and word list preference that you just created.

    In this example, `my_policy` is the name of our policy for `mylex` and `mywordlist`

    ![](./images/drop-policy.png " ")

    ![](./images/create-policy.png " ")

## **STEP 6**: Model Building

### Build Attribute Importance Model

Now, let's build an attribute importance model using both structured and unstructured (wine reviews) data.

***sentence below is confusing***

1. Give the `ALGO_NAME`. We're using a minimum descriptor length for attribute importance. Then specify the text policy name  you created i.e , the maximum of features you want your model to use, the default value is 3000 which defines the maximum number of features to use from the document set.

    Note that it just took 54 sec to show the actual words from the 130k records of unstructured text mining data.

    ![](./images/attribute-model.png " ")

2. Once you run the attribute importance model, notice that we have our attribute importance ranked based on the ascending order of the rank i.e by price, province, variety etc.

    Here specific words like palate, wine, aromas, acidity, finish, rich etc are the tokens from the table that influence the attributes to get a rich wine.

    The attribute importance values are the coefficients that show how strong each individual word is.

    In this example, PALATE is ranked 4 from description with coefficient value 0.0311229.

    ![](./images/rank.png " ")

### Build Classification Model

As we built our attribute importance model, we will build a classification model using both structured and unstructured (wine\_reviews) data.

3. Build a supervised learning classification model - "Wine\_CLASS\_MODEL\_SVM" that predicts good wine (GT\_90\_POINTS) using Oracle Machine Learning Support Vector Machine Algorithm.

    ![](./images/svm-classification-model.png" ")

## **STEP 7**: Model Evaluation

Now that we have built a machine learning model, let's evaluate the model.

1. We score the data by applying the model we just created - "Wine\_CLASS\_MODEL\_SVM" to the test data - "WINE\_TEST\_DATA" and the results are stored in - "Wine\_APPLY\_RESULT".

    To see how good or bad the model is, we compute a lift chart using COMPUTE\_LIFT to see how our model is performing against the RANDOM\_GUESS.

    ![](./images/evalute-model.png " ")

    ![](./images/lift-chart.png " ")

2. Here is the result of the data mining model we just created with the TARGET\_VALUE, ATTRIBUTE\_NAME, COEFFICIENT, REVERSED\_COEFFICIENT.

    ![](./images/ml-model-output.png " ")

## **STEP 8**: Model Deployment

Now let's apply the model to specific data points.

1. Explore the wines that are predicted to be good wines based on the classification we did i.e., greater than 90 points and less than 90 points. Each row in our test table displays the predicted result.

    ![](./images/model-deployement.png " ")

2. Focusing on the wines that have been predicted to be the good wines i.e., greater than 90 points and comparing them with the bad wines i.e., lower than 90 points, we are applying our model result on the actual dataset and then predicting it.

    As we are applying the model, we get a prediction result of: which wine falls into which category, it's probability of being greater than 90, the actual wine description and country along with a few other parameters.

    For example, the first record in the screenshot, ID - 127518 shows the prediction to be greater than 90 points and has the probability - 0.905 (approximately 90%) in which it is greater then 90 points. Notice that the description for this record mentions all the characteristics of a good wine.

    ![](./images/actual-data-result.png " ")

3. As we wanted inexpensive wines, this graphs shows 1000 good wines that are less than $15 and with predictions of being greater than 90 points by countries, based on our data set and model.

    From the graph, notice that France has a good number of wines that are good as well as cheap and then US followed by Italy and Chile.

    ![](./images/france.png " ")

    ![](./images/usa.png " ")


4. In order to investigate wine insights and predictions a bit further using Oracle Analytics Cloud, we remove any previous "WinePredictions" table.

    ![](./images/remove-prediction-table.png " ")

5. Create a new "WinePredictions" table in ADW to be accessed by Oracle Analytics Cloud and run our model on the entire data set to gain more visual insights.

    ![](./images/create-table-oac.png " ")

6. View the "WinePredictions" table created.

    ![](./images/show-table-oac.png " ")

You may now [proceed to the next lab](#next).


## Acknowledgements
* **Author** - Charlie Berger & Dhvani Sheth, Data Mining and Advanced Analytics
* **Contributors** -  Anoosha Pilli & Didi Han, Database Product Management
* **Last Updated By/Date** - Didi Han, Database Product Management,  March 2021
