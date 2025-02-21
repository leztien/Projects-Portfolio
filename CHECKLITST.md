# Project Checklist 
(based on Aurélien Géron's [checklist](https://github.com/ageron/handson-ml3/blob/main/ml-project-checklist.md)))



## The usual "shell preparations":
```shell
# setting up git
$ sudo apt update; sudo apt install git
$ git config --global user.name "My Name"
$ git config --global user.email "email@mail.com"

# setting up a repo
$ cd Projects
$ mkdir project
$ cd project
$ git init
$ git branch -m main  # -m is ALMOST the same as -M
$ git config --global init.defaultBranch main   # alternative to the above line - will name new branches as main

$ touch .gitignore; echo -e "*.log\n.venv/\ndata/" > .gitignore
# or write the fileas and folders to ignore in here: .git/info/exclude
$ mkdir data src assets
$ touch README.md requirements.txt
$ echo "# Project" > README.md
$ vim requirements.txt
$ cat requirements.txt

# setting up an environment
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt
$ pip list
$ git add .  # do not do this!
$ git reset  # unstage all files
$ ls -l
$ git add README.md requirements.txt .gitignore
$ git commit -m "my first commit"

# create a new repo on GitHub
$ git branch -M main  # if you haven't renamed it before
$ git remote add origin https://github.com/leztien/project.git  #if HTTPS and token
$ git remote add origin git@github.com:leztien/project.git  #if SSH
$ git push -u origin main  # -u == --set-upstream
$ git push -u -f origin master  # if the above doesn't work
$ code .

# routine commits
$ git add README.md requirements.txt
$ git commit -m "comment"
$ git push

# deployment
$ pip freeze > requirements.txt
$ copy the contents of this folder into a new 'deployment' folder and cd into it
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt
$ uvicorn main:app --reload
```

<br>


### There are eight main steps:  

1. Frame the problem and look at the big picture.
2. Get the data.
3. Explore the data to gain insights.
4. Prepare the data to better expose the underlying data patterns to Machine Learning algorithms. <br>
4-5. Baseline model. <br>
5. Explore many different models and short-list the best ones. <br>
5-6. Error Analysis. <br>
6. Fine-tune your models and combine them into a great solution.
7. Present your solution.
8. Launch, monitor, and maintain your system.  

  

## 1. Frame the problem and look at the big picture

1. Define the objective in business terms.  
2. How will your solution be used?  
3. What are the current solutions/workarounds (if any)?  
4. How should you frame this problem (supervised/unsupervised, online/offline, etc.)  
5. How should performance be measured?  
6. Is the performance measure aligned with the business objective?  
7. What would be the minimum performance needed to reach the business objective?  
8. What are comparable problems? Can you reuse experience or tools?  
9. Is human expertise available?  
10. How would you solve the problem manually?  
11. List the assumptions you or others have made so far.  
12. Verify assumptions if possible.  



## 2. Get the data    

1. List the data you need and how much you need.  
2. Find and document where you can get that data.  
3. Check how much space it will take.  
4. Check legal obligations, and get the authorization if necessary.  
5. Get access authorizations.  
6. Create a workspace (with enough storage space).  
7. Get the data.
8. Make a custom 'get_data' or 'fetch_data' function.
9. Convert the data to a format you can easily manipulate (without changing the data itself).  
10. Ensure sensitive information is deleted or protected (e.g., anonymized). 
11. Check the size and type of data (time series, sample, geographical, etc.).  
12. Sample a test set, put it aside, and never look at it (no data snooping!).    



## 3. Explore the data (EDA)

Note: try to get insights from a field expert for these steps.  

1. Create a copy of the data for exploration (sampling it down to a manageable size if necessary).
2. Create a Jupyter notebook to keep record of your data exploration.
3. For supervised learning tasks, identify the target attribute(s).
4. Study each attribute and its characteristics:  
    - Unique values bzw df.describe() - if continuous  
    - Type (categorical, int/float, bounded/unbounded, text, structured, etc.)
    - % of missing values  
    - Noisiness and type of noise (stochastic, outliers, rounding errors, etc.)
    - Outlier analysis
    - Possibly useful for the task?  
    - Type of distribution (Gaussian, uniform, logarithmic, etc.)
5. Visualize the data.
    - Bar charts for categorical features  
6. Study the correlations between attributes. (and with the target?)  
7. Study how you would solve the problem manually.  
8. Identify the promising transformations you may want to apply.
9. Any feature engineering ideas? (interaction, ratios, aggregations).  
10. Identify extra data that would be useful (go back to "Get the Data" on page 502).  
11. Document what you have learned.
12. optional: come up with some hypothesis tests



## 4. Prepare the data  

Notes:    
- Work on copies of the data (keep the original dataset intact).  
- Write functions for all data transformations you apply, for five reasons:  
    - So you can easily prepare the data the next time you get a fresh dataset  
    - So you can apply these transformations in future projects  
    - To clean and prepare the test set  
    - To clean and prepare new data instances  
    - To make it easy to treat your preparation choices as hyperparameters  

1. Data cleaning:  
    - Fix or remove outliers (optional).  
    - Fill in missing values (e.g., with zero, mean, median...) or drop their rows (or columns).  
2. Feature selection (optional):  
    - Drop the attributes that provide no useful information for the task.  
3. Feature engineering, where appropriate:  
    - Discretize continuous features.  
    - Decompose features (e.g., categorical, date/time, etc.).  
    - Add promising transformations of features (e.g., log(x), sqrt(x), x^2, etc.).
    - Aggregate features into promising new features (interaction, ratios, aggregations).    
4. Feature scaling: standardize or normalize features.  


## 4-5. Baseline model


## 5. Short-list promising models  

Notes: 
- If the data is huge, you may want to sample smaller training sets so you can train many different models in a reasonable time (be aware that this penalizes complex models such as large neural nets or Random Forests).  
- Once again, try to automate these steps as much as possible.    

1. Train many quick and dirty models from different categories (e.g., linear, naive, Bayes, SVM, Random Forests, neural net, etc.) using standard parameters.  
2. Measure and compare their performance.  
    - For each model, use N-fold cross-validation and compute the mean and standard deviation of their performance. 
3. Analyze the most significant variables for each algorithm.  
4. Analyze the types of errors the models make (What data would a human have used to avoid these errors?)
5. Have a quick round of feature selection and engineering.  
6. Have one or two more quick iterations of the five previous steps.



## 5-6. Error Analysis (see below "Detailed Error Analysis")

After evaluating the model, conduct error analysis to understand where and why the model is making mistakes.
This step involves examining the errors made by the model on the validation or test data.

Steps in Error Analysis:
- Collecting Errors: Gather a sample of instances where the model's predictions were incorrect.
- Error Categories: Categorize errors into types (e.g., false positives, false negatives).
- Feature Analysis: Analyze features of instances where errors were made. Look for patterns.
- Confusion Matrix: Create a confusion matrix to see the overall performance across classes.
- Precision-Recall Curves: Plot these curves to understand precision and recall trade-offs.
- Error Rates by Subgroup: Analyze error rates across different subgroups, if applicable.
- Sample Analysis: Look at individual examples to understand specific instances of errors.
(see the deteiled Error Analysis below)
Short-list the top three to five most promising models, preferring models that make different types of errors.  



## 6. Fine-Tune the System / Model Improvement 
Notes:  
- You will want to use as much data as possible for this step, especially as you move toward the end of fine-tuning.   
- As always automate what you can.    

1. Fine-tune the hyperparameters using cross-validation.  
    - Treat your data transformation choices as hyperparameters, especially when you are not sure about them (e.g., should I replace missing values with zero or the median value? Or just drop the rows?).  
    - Unless there are very few hyperparameter values to explore, prefer random search over grid search. If training is very long, you may prefer a Bayesian optimization approach (e.g., using a Gaussian process priors, as described by Jasper Snoek, Hugo Larochelle, and Ryan Adams ([https://goo.gl/PEFfGr](https://goo.gl/PEFfGr)))  
2. Try Ensemble methods. Combining your best models will often perform better than running them individually.
    - first unite your classifiers into a soft voting classifier, and then do threshold optimisation for your metric (e.g.f1)
3. Calibrate probabilities (for classification with probabilities)
    - https://scikit-learn.org/stable/modules/calibration.html
    - https://chat.openai.com/share/f7b91fab-cca8-49ea-af44-9f2f469f9d9f
4. Once you are confident about your final model, measure its performance on the test set to estimate the generalization error.
5. Don't tweak your model after measuring the generalization error: you would just start overfitting the test set.



## 7. Present your solution  
1. Document what you have done.  
2. Create a nice presentation.  
    - Make sure you highlight the big picture first.  
3. Explain why your solution achieves the business objective.  
4. Don't forget to present interesting points you noticed along the way.  
    - Describe what worked and what did not.  
    - List your assumptions and your system's limitations.  
5. Ensure your key findings are communicated through beautiful visualizations or easy-to-remember statements (e.g., "the median income is the number-one predictor of housing prices").  



## 8. Launch!  
1. Get your solution ready for production (plug into production data inputs, write unit tests, etc.).  
2. Write monitoring code to check your system's live performance at regular intervals and trigger alerts when it drops.  
    - Beware of slow degradation too: models tend to "rot" as data evolves.   
    - Measuring performance may require a human pipeline (e.g., via a crowdsourcing service).  
    - Also monitor your inputs' quality (e.g., a malfunctioning sensor sending random values, or another team's output becoming stale). This is particularly important for online learning systems.  
3. Retrain your models on a regular basis on fresh data (automate as much as possible).  


<br><br>


# Detailed Error Analysis (from ChatGPT):

Performing an error analysis is an essential step in understanding a model's performance and the types of errors it makes. Here are steps to perform an error analysis:
1. Collect Predictions and True Labels

    Start with your model's predictions on a validation or test set.
    Collect the true labels corresponding to these predictions.

2. Confusion Matrix

    Construct a confusion matrix. This is particularly useful for classification tasks.
    For each class, the confusion matrix shows how many samples were correctly classified and how many were misclassified into other classes.
    This helps in understanding which classes are often confused with each other.

3. Calculate Metrics

    Calculate standard evaluation metrics like accuracy, precision, recall, and F1-score.
    These metrics provide an overall view of the model's performance but can be limited in understanding specific errors.

4. Analyze Misclassifications

    Look at individual samples that were misclassified.
    Examine the features, context, or patterns of these samples to identify commonalities.
    Pay attention to edge cases or samples with ambiguous labels.

5. Class-wise Analysis

    Perform a class-wise analysis:
        For each class, look at precision, recall, and F1-score.
        Identify which classes have low precision (false positive rate) or low recall (false negative rate).
        Determine if there's a class imbalance affecting the model's performance.

6. Visualizations

    Use visualizations to aid in understanding errors:
        Plotting actual vs. predicted values.
        ROC curves or precision-recall curves for binary classification.
        Distribution of scores or probabilities for correct and incorrect predictions.
        Feature importance or contribution to misclassifications.

7. Error Patterns

    Look for recurring error patterns:
        Are there specific types of inputs (e.g., images, sentences) that are consistently misclassified?
        Are there particular feature values or combinations that lead to errors?

8. Human Review

    In some cases, involve human reviewers to:
        Check misclassified samples to verify if the model's predictions are reasonable.
        Annotate the reason for misclassifications.
        Provide insights that might not be apparent from automated analysis.

9. Addressing Errors

    Based on the analysis, consider strategies to address specific errors:
        Collect more data for underrepresented classes.
        Engineer new features that might help the model.
        Adjust the model's hyperparameters.
        Use techniques like ensembling or different models for specific classes.
