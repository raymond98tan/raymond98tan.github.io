---
layout: post
title: Helping Families Find Homes
subtitle: by Raymond Tan
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [project]
---

[Family Promise of Spokane](https://www.familypromiseofspokane.org/) is a non-profit organization tackling the growing incidence of family homelessness in the Spokane, Washington area in an effort to provide resources to those in danger of not having their most basic living needs met. The shelter addresses family homelessness through every stage from prevention (rental assistance, transportation), shelter (hospitality and case management), to stabilization (housing, financial capability, career pathways).

![Resources](/assets/img/Prevention_shelter_stabilization-1.jpg)

As Family Promise of Spokane has grown over the years to meet the needs of guests, their outdated intake forms and resource management have struggled to keep up with the shelter’s growth. To fight against the time-consuming labor requirements and insensitivity to the stresses a written intake form puts on new guests, my team has worked to develop a digital intake solution to address the out-of-date system currently in place, as well as a dashboard management system.

![Washington Homelessness](/assets/img/Washington_Homelessness_Stat.JPG)

Not only did we provide case managers and supervisors the ability to process guests more efficiently, but we also implemented machine learning into this system to help Case Managers better distribute resources to families. Specifically, we would build a machine learning application to predict the exit destination a family is most likely to go to after transitioning from the shelter. Entering into the project as an API builder and data scientist, I was tasked with working alongside my cross-functional team to generate interpretable data visualizations from our predictive model, showcasing the reasoning behind why a family is predicted to exit where classified.


### Taking a step back to revisit the big picture

When approaching the task of creating data visualizations, my team of three data scientists all produced our own individual visualizations and met up to discuss which ideas we liked. However, during our meeting, we realized that the visualizations that we created should refer back to the stakeholder’s needs. Instead of creating any visualizations we could from the model, we should focus more on visualizations that would be beneficial for the end-user, the Case Manager, especially for model features that are within their control to influence the outcome. With this in mind, we began to focus on just the top three most important features and their visualizations.

Implementing the code in the API, we ran into many challenges. We first discussed and wrote pseudo code to outline our thought process and from there, translated it into actual code. The idea was that we’d run the prediction model on a guest, get the top three features for that model, run a for-loop on the dictionary of top features, turning it into a dataframe that can then be passed in to create bar-plot visualizations and transcribed into JSON. Lastly, they are stored into a list and returned. However, we weren’t able to test our code since we were missing environment credentials from the previous team. Once we received those credentials to access the database, we ran into more issues within the code, specifically with the loop that auto-generates the visualizations. To tackle the problem, we moved into a Google Colab notebook to specifically address what was wrong and make the changes into the actual file once solved. The final challenge in our code implementation was receiving a ‘Coroutine’ object when trying to get the top features, when we should have been receiving a dictionary. Upon further research, we found out that coroutines are generalizations of subroutines and are used for cooperative multitasking. We were receiving this object because of our asynchronous functions, so we created a clone of ml.py with a normal function definition to draw the dictionary from. After going through all these hurdles, we were able to finally output the JSON object for our visualizations.


### My End, Another's Beginning
By the end of the month, we were able to improve upon the previous model’s 70% accuracy to 75% accuracy using Catboost. We built features into the database so that we could use our new model. We drew Shapley plot visualizations detailing feature importances for each exit classification. Improving the model and getting the visualizations working was a huge step this month, as it will allow for Case Managers to better distinguish which guests need more attention and identify the resources that may be most beneficial for them.

![Shapley API Code](/assets/img/shapcode1.jpg)

This was the code used in the API to generate the Shapley plot visualization. It was used alongside a function which passes in the guest’s information and converts the generated Plotly into a JSON object to be displayed on the front end. The function first takes the row of the guest that is passed in and predicts their exit classification using the pickled Catboost model. TreeExplainer is then used on the model to compute its Shap values, removing all sampling-based estimation variance and no longer skewing results due to dependencies between features. We then store the feature names and values for the guest in a sorted series. This information is then used as the data shown on the Plotly chart. Originally, we displayed the visualizations using Pyplot, but we converted it to Plotly so that we could more easily retrieve the JSON for the front end.

![Shapley Visualization](/assets/img/Shap.jpg)

On the web development side of things, they were able to build the intake forms and dashboards, along with many of the gritty details necessary for smooth and efficient use of the product. The team made a ton of progress on the project this month, but we still have ways to go from a final product that can be beta tested. Moving forward, we are leaving the next data science team with the tasks of deploying the new model onto AWS, as we ran into Docker conflicts that weren’t able to be fixed in time, and actually deploying the Shapley plots so the front end can display them for additional interpretability.

This has been the most comprehensive, big team project that I’ve been involved with so far in software development. It was insightful experiencing the process of product production from the planning, to designing, to building. There were countless team meetings, stakeholder meetings, and hours spent wrestling various technical issues. However, knowing that the lives of families are being changed through the work that we’re doing is worth it all. Seeing how everyone in the team worked and communicated has influenced my way of thinking and approach to problem solving and teamwork. I am grateful that I got to work with such an amazing, lively team of data scientists and web developers. I will strive to become more well rounded in my skill set in my future career and always hold onto the experiences I had within this project.


