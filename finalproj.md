---
layout: default
---
**[Home](https://yvesmango.github.io/) >> [Projects](https://yvesmango.github.io/projects) >> [R Kaggle Survey 2022](https://yvesmango.github.io/r-kaggle-survey-2022/) >> Jupyter Notebook**

This is an [R Markdown](http://rmarkdown.rstudio.com) Notebook. When you
execute code within the notebook, the results appear beneath the code.

Try executing this chunk by clicking the *Run* button within the chunk
or by placing your cursor inside it and pressing *Cmd+Shift+Enter*.

Loading package and dependencies:

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(ggplot2)
    library(data.table)

    ## data.table 1.14.4 using 1 threads (see ?getDTthreads).  Latest news: r-datatable.com

    ## **********
    ## This installation of data.table has not detected OpenMP support. It should still work but in single-threaded mode.
    ## This is a Mac. Please read https://mac.r-project.org/openmp/. Please engage with Apple and ask them for support. Check r-datatable.com for updates, and our Mac instructions here: https://github.com/Rdatatable/data.table/wiki/Installation. After several years of many reports of installation problems on Mac, it's time to gingerly point out that there have been no similar problems on Windows or Linux.
    ## **********

    ## 
    ## Attaching package: 'data.table'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last

    library(tidyr)
    library(forcats)
    library(stringr)

Add a new chunk by clicking the *Insert Chunk* button on the toolbar or
by pressing *Cmd+Option+I*.

When you save the notebook, an HTML file containing the code and output
will be saved alongside it (click the *Preview* button or press
*Cmd+Shift+K* to preview the HTML file).

The preview shows you a rendered HTML copy of the contents of the
editor. Consequently, unlike *Knit*, *Preview* does not run any R code
chunks. Instead, the output of the chunk when it was last run in the
editor is displayed.

    df1 <- read.csv('./kagglesurvey/kaggle_survey_2022_responses.csv')

## Section A. Gender Dynamics

### Part 1. Yearly compensation

#### Data prepping and manipulation

    head(df1$Q5)

    ## [1] "Are you currently a student? (high school, university, or graduate)"
    ## [2] "No"                                                                 
    ## [3] "No"                                                                 
    ## [4] "Yes"                                                                
    ## [5] "No"                                                                 
    ## [6] "Yes"

     df1 <- df1[-1,]
     head(df1$Q5)

    ## [1] "No"  "No"  "Yes" "No"  "Yes" "Yes"

    ## removing the first row

    table(df1$Q5)

    ## 
    ##    No   Yes 
    ## 12036 11961

    prop.table(table(df1$Q5))

    ## 
    ##        No       Yes 
    ## 0.5015627 0.4984373

    #proportion of non-students to students 

    table(df1$Q3)

    ## 
    ##                     Man               Nonbinary       Prefer not to say Prefer to self-describe 
    ##                   18266                      78                     334                      33 
    ##                   Woman 
    ##                    5286

    as.data.frame(prop.table(table(df1$Q3)))

    ##                      Var1        Freq
    ## 1                     Man 0.761178481
    ## 2               Nonbinary 0.003250406
    ## 3       Prefer not to say 0.013918406
    ## 4 Prefer to self-describe 0.001375172
    ## 5                   Woman 0.220277535

    #ratio of gender

    table(df1$Q7)

    ## < table of extent 0 >

    # platform poularity

    table(df1$Q12_1)

    ## 
    ##        Python 
    ##   5344  18653

    data.frame(prop.table(table(df1$Q12_1)))

    ##     Var1      Freq
    ## 1        0.2226945
    ## 2 Python 0.7773055

    df_index <- read.csv('./kagglesurvey/kaggle_survey_2022_responses.csv', header=0)

    df_index[1:2,]

    ##                      V1                          V2                                     V3
    ## 1 Duration (in seconds)                          Q2                                     Q3
    ## 2 Duration (in seconds) What is your age (# years)? What is your gender? - Selected Choice
    ##                                          V4                                                                  V5
    ## 1                                        Q4                                                                  Q5
    ## 2 In which country do you currently reside? Are you currently a student? (high school, university, or graduate)
    ##                                                                                                                          V6
    ## 1                                                                                                                      Q6_1
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Coursera
    ##                                                                                                                     V7
    ## 1                                                                                                                 Q6_2
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - edX
    ##                                                                                                                                      V8
    ## 1                                                                                                                                  Q6_3
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Kaggle Learn Courses
    ##                                                                                                                          V9
    ## 1                                                                                                                      Q6_4
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - DataCamp
    ##                                                                                                                        V10
    ## 1                                                                                                                     Q6_5
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Fast.ai
    ##                                                                                                                        V11
    ## 1                                                                                                                     Q6_6
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Udacity
    ##                                                                                                                      V12
    ## 1                                                                                                                   Q6_7
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Udemy
    ##                                                                                                                                  V13
    ## 1                                                                                                                               Q6_8
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - LinkedIn Learning
    ##                                                                                                                                                                                       V14
    ## 1                                                                                                                                                                                    Q6_9
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Cloud-certification programs (direct from AWS, Azure, GCP, or similar)
    ##                                                                                                                                                                      V15
    ## 1                                                                                                                                                                  Q6_10
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - University Courses (resulting in a university degree)
    ##                                                                                                                     V16
    ## 1                                                                                                                 Q6_11
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - None
    ##                                                                                                                      V17
    ## 1                                                                                                                  Q6_12
    ## 2 On which platforms have you begun or completed data science courses? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                                        V18
    ## 1                                                                                                                                                                     Q7_1
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - University courses
    ##                                                                                                                                                                                         V19
    ## 1                                                                                                                                                                                      Q7_2
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - Online courses (Coursera, EdX, etc)
    ##                                                                                                                                                                                                   V20
    ## 1                                                                                                                                                                                                Q7_3
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - Social media platforms (Reddit, Twitter, etc)
    ##                                                                                                                                                                                            V21
    ## 1                                                                                                                                                                                         Q7_4
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - Video platforms (YouTube, Twitch, etc)
    ##                                                                                                                                                                                           V22
    ## 1                                                                                                                                                                                        Q7_5
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - Kaggle (notebooks, competitions, etc)
    ##                                                                                                                                                                                        V23
    ## 1                                                                                                                                                                                     Q7_6
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - None / I do not study data science
    ##                                                                                                                                                           V24
    ## 1                                                                                                                                                        Q7_7
    ## 2 What products or platforms did you find to be most helpful when you first started studying data science?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                               V25
    ## 1                                                                                                              Q8
    ## 2 What is the highest level of formal education that you have attained or plan to attain within the next 2 years?
    ##                                                                                               V26
    ## 1                                                                                              Q9
    ## 2 Have you ever published any academic research (papers, preprints, conference proceedings, etc)?
    ##                                                                                                                                                      V27
    ## 1                                                                                                                                                  Q10_1
    ## 2 Did your research make use of machine learning? - Yes, the research made advances related to some novel machine learning method (theoretical research)
    ##                                                                                                                             V28
    ## 1                                                                                                                         Q10_2
    ## 2 Did your research make use of machine learning? - Yes, the research made use of machine learning as a tool (applied research)
    ##                                                    V29
    ## 1                                                Q10_3
    ## 2 Did your research make use of machine learning? - No
    ##                                                                 V30
    ## 1                                                               Q11
    ## 2 For how many years have you been writing code and/or programming?
    ##                                                                                                            V31
    ## 1                                                                                                        Q12_1
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Python
    ##                                                                                                       V32
    ## 1                                                                                                   Q12_2
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - R
    ##                                                                                                         V33
    ## 1                                                                                                     Q12_3
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - SQL
    ##                                                                                                       V34
    ## 1                                                                                                   Q12_4
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - C
    ##                                                                                                        V35
    ## 1                                                                                                    Q12_5
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - C#
    ##                                                                                                         V36
    ## 1                                                                                                     Q12_6
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - C++
    ##                                                                                                          V37
    ## 1                                                                                                      Q12_7
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Java
    ##                                                                                                                V38
    ## 1                                                                                                            Q12_8
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Javascript
    ##                                                                                                          V39
    ## 1                                                                                                      Q12_9
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Bash
    ##                                                                                                         V40
    ## 1                                                                                                    Q12_10
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - PHP
    ##                                                                                                            V41
    ## 1                                                                                                       Q12_11
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - MATLAB
    ##                                                                                                           V42
    ## 1                                                                                                      Q12_12
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Julia
    ##                                                                                                        V43
    ## 1                                                                                                   Q12_13
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Go
    ##                                                                                                          V44
    ## 1                                                                                                     Q12_14
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - None
    ##                                                                                                           V45
    ## 1                                                                                                      Q12_15
    ## 2 What programming languages do you use on a regular basis? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                          V46
    ## 1                                                                                                                                                      Q13_1
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice - JupyterLab 
    ##                                                                                                                                                        V47
    ## 1                                                                                                                                                    Q13_2
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  RStudio 
    ##                                                                                                                                                              V48
    ## 1                                                                                                                                                          Q13_3
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  Visual Studio 
    ##                                                                                                                                                                            V49
    ## 1                                                                                                                                                                        Q13_4
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  Visual Studio Code (VSCode) 
    ##                                                                                                                                                        V50
    ## 1                                                                                                                                                    Q13_5
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  PyCharm 
    ##                                                                                                                                                         V51
    ## 1                                                                                                                                                     Q13_6
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -   Spyder  
    ##                                                                                                                                                            V52
    ## 1                                                                                                                                                        Q13_7
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -   Notepad++  
    ##                                                                                                                                                               V53
    ## 1                                                                                                                                                           Q13_8
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -   Sublime Text  
    ##                                                                                                                                                              V54
    ## 1                                                                                                                                                          Q13_9
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -   Vim / Emacs  
    ##                                                                                                                                                       V55
    ## 1                                                                                                                                                  Q13_10
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  MATLAB 
    ##                                                                                                                                                                V56
    ## 1                                                                                                                                                           Q13_11
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice -  Jupyter Notebook
    ##                                                                                                                                                       V57
    ## 1                                                                                                                                                  Q13_12
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice - IntelliJ
    ##                                                                                                                                                   V58
    ## 1                                                                                                                                              Q13_13
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                                    V59
    ## 1                                                                                                                                               Q13_14
    ## 2 Which of the following integrated development environments (IDE's) do you use on a regular basis?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                        V60
    ## 1                                                                                                                    Q14_1
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Kaggle Notebooks
    ##                                                                                                                      V61
    ## 1                                                                                                                  Q14_2
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - Colab Notebooks
    ##                                                                                                                      V62
    ## 1                                                                                                                  Q14_3
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - Azure Notebooks
    ##                                                                                                                   V63
    ## 1                                                                                                               Q14_4
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Code Ocean 
    ##                                                                                                                          V64
    ## 1                                                                                                                      Q14_5
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  IBM Watson Studio 
    ##                                                                                                                                V65
    ## 1                                                                                                                            Q14_6
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Amazon Sagemaker Studio 
    ##                                                                                                                                    V66
    ## 1                                                                                                                                Q14_7
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Amazon Sagemaker Studio Lab 
    ##                                                                                                                             V67
    ## 1                                                                                                                         Q14_8
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Amazon EMR Notebooks 
    ##                                                                                                                                        V68
    ## 1                                                                                                                                    Q14_9
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - Google Cloud Vertex AI Workbench 
    ##                                                                                                                     V69
    ## 1                                                                                                                Q14_10
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - Hex Workspaces
    ##                                                                                                                           V70
    ## 1                                                                                                                      Q14_11
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Noteable Notebooks 
    ##                                                                                                                                           V71
    ## 1                                                                                                                                      Q14_12
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Databricks Collaborative Notebooks 
    ##                                                                                                                           V72
    ## 1                                                                                                                      Q14_13
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Deepnote Notebooks 
    ##                                                                                                                           V73
    ## 1                                                                                                                      Q14_14
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice -  Gradient Notebooks 
    ##                                                                                                           V74
    ## 1                                                                                                      Q14_15
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - None
    ##                                                                                                            V75
    ## 1                                                                                                       Q14_16
    ## 2 Do you use any of the following hosted notebook products?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                          V76
    ## 1                                                                                                                                      Q15_1
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Matplotlib 
    ##                                                                                                                                       V77
    ## 1                                                                                                                                   Q15_2
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Seaborn 
    ##                                                                                                                                                       V78
    ## 1                                                                                                                                                   Q15_3
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Plotly / Plotly Express 
    ##                                                                                                                                                V79
    ## 1                                                                                                                                            Q15_4
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Ggplot / ggplot2 
    ##                                                                                                                                     V80
    ## 1                                                                                                                                 Q15_5
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Shiny 
    ##                                                                                                                                     V81
    ## 1                                                                                                                                 Q15_6
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  D3 js 
    ##                                                                                                                                      V82
    ## 1                                                                                                                                  Q15_7
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Altair 
    ##                                                                                                                                     V83
    ## 1                                                                                                                                 Q15_8
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Bokeh 
    ##                                                                                                                                          V84
    ## 1                                                                                                                                      Q15_9
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Geoplotlib 
    ##                                                                                                                                                V85
    ## 1                                                                                                                                           Q15_10
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Leaflet / Folium 
    ##                                                                                                                                     V86
    ## 1                                                                                                                                Q15_11
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Pygal 
    ##                                                                                                                                        V87
    ## 1                                                                                                                                   Q15_12
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Dygraphs 
    ##                                                                                                                                           V88
    ## 1                                                                                                                                      Q15_13
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice -  Highcharter 
    ##                                                                                                                                  V89
    ## 1                                                                                                                             Q15_14
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                   V90
    ## 1                                                                                                                              Q15_15
    ## 2 Do you use any of the following data visualization libraries on a regular basis?  (Select all that apply) - Selected Choice - Other
    ##                                                          V91
    ## 1                                                        Q16
    ## 2 For how many years have you used machine learning methods?
    ##                                                                                                                                             V92
    ## 1                                                                                                                                         Q17_1
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -   Scikit-learn 
    ##                                                                                                                                           V93
    ## 1                                                                                                                                       Q17_2
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -   TensorFlow 
    ##                                                                                                                                     V94
    ## 1                                                                                                                                 Q17_3
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Keras 
    ##                                                                                                                                       V95
    ## 1                                                                                                                                   Q17_4
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  PyTorch 
    ##                                                                                                                                       V96
    ## 1                                                                                                                                   Q17_5
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Fast.ai 
    ##                                                                                                                                       V97
    ## 1                                                                                                                                   Q17_6
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Xgboost 
    ##                                                                                                                                        V98
    ## 1                                                                                                                                    Q17_7
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  LightGBM 
    ##                                                                                                                                        V99
    ## 1                                                                                                                                    Q17_8
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  CatBoost 
    ##                                                                                                                                    V100
    ## 1                                                                                                                                 Q17_9
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Caret 
    ##                                                                                                                                         V101
    ## 1                                                                                                                                     Q17_10
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Tidymodels 
    ##                                                                                                                                  V102
    ## 1                                                                                                                              Q17_11
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  JAX 
    ##                                                                                                                                                V103
    ## 1                                                                                                                                            Q17_12
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  PyTorch Lightning 
    ##                                                                                                                                          V104
    ## 1                                                                                                                                      Q17_13
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice -  Huggingface 
    ##                                                                                                                                 V105
    ## 1                                                                                                                             Q17_14
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice - None
    ##                                                                                                                                  V106
    ## 1                                                                                                                              Q17_15
    ## 2 Which of the following machine learning frameworks do you use on a regular basis? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                             V107
    ## 1                                                                                                                                          Q18_1
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Linear or Logistic Regression
    ##                                                                                                                                                V108
    ## 1                                                                                                                                             Q18_2
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Decision Trees or Random Forests
    ##                                                                                                                                                                   V109
    ## 1                                                                                                                                                                Q18_3
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Gradient Boosting Machines (xgboost, lightgbm, etc)
    ##                                                                                                                                   V110
    ## 1                                                                                                                                Q18_4
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Bayesian Approaches
    ##                                                                                                                                       V111
    ## 1                                                                                                                                    Q18_5
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Evolutionary Approaches
    ##                                                                                                                                                 V112
    ## 1                                                                                                                                              Q18_6
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Dense Neural Networks (MLPs, etc)
    ##                                                                                                                                             V113
    ## 1                                                                                                                                          Q18_7
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Convolutional Neural Networks
    ##                                                                                                                                               V114
    ## 1                                                                                                                                            Q18_8
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Generative Adversarial Networks
    ##                                                                                                                                         V115
    ## 1                                                                                                                                      Q18_9
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Recurrent Neural Networks
    ##                                                                                                                                                       V116
    ## 1                                                                                                                                                   Q18_10
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Transformer Networks (BERT, gpt-3, etc)
    ##                                                                                                                                                    V117
    ## 1                                                                                                                                                Q18_11
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Autoencoder Networks (DAE, VAE, etc)
    ##                                                                                                                                     V118
    ## 1                                                                                                                                 Q18_12
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Graph Neural Networks
    ##                                                                                                                    V119
    ## 1                                                                                                                Q18_13
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - None
    ##                                                                                                                     V120
    ## 1                                                                                                                 Q18_14
    ## 2 Which of the following ML algorithms do you use on a regular basis? (Select all that apply): - Selected Choice - Other
    ##                                                                                                                                                                                 V121
    ## 1                                                                                                                                                                              Q19_1
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - General purpose image/video tools (PIL, cv2, skimage, etc)
    ##                                                                                                                                                                          V122
    ## 1                                                                                                                                                                       Q19_2
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Image segmentation methods (U-Net, Mask R-CNN, etc)
    ##                                                                                                                                                                        V123
    ## 1                                                                                                                                                                     Q19_3
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Object detection methods (YOLOv6, RetinaNet, etc)
    ##                                                                                                                                                                                                                                           V124
    ## 1                                                                                                                                                                                                                                        Q19_4
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Image classification and other general purpose networks (VGG, Inception, ResNet, ResNeXt, NASNet, EfficientNet, etc)
    ##                                                                                                                                                          V125
    ## 1                                                                                                                                                       Q19_5
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Generative Networks (GAN, VAE, etc)
    ##                                                                                                                                                                                    V126
    ## 1                                                                                                                                                                                 Q19_6
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Vision transformer networks (ViT, DeiT, BiT, BEiT, Swin, etc)
    ##                                                                                                                           V127
    ## 1                                                                                                                        Q19_7
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                            V128
    ## 1                                                                                                                         Q19_8
    ## 2 Which categories of computer vision methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                                                               V129
    ## 1                                                                                                                                                                                            Q20_1
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Word embeddings/vectors (GLoVe, fastText, word2vec)
    ##                                                                                                                                                                                                  V130
    ## 1                                                                                                                                                                                               Q20_2
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Encoder-decoder models (seq2seq, vanilla transformers)
    ##                                                                                                                                                                                  V131
    ## 1                                                                                                                                                                               Q20_3
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Contextualized embeddings (ELMo, CoVe)
    ##                                                                                                                                                                                                 V132
    ## 1                                                                                                                                                                                              Q20_4
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Transformer language models (GPT-3, BERT, XLnet, etc)
    ##                                                                                                                                                V133
    ## 1                                                                                                                                             Q20_5
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                                 V134
    ## 1                                                                                                                                              Q20_6
    ## 2 Which of the following natural language processing (NLP) methods do you use on a regular basis?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                          V135
    ## 1                                                                                                                                       Q21_1
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -   TensorFlow Hub 
    ##                                                                                                                                      V136
    ## 1                                                                                                                                   Q21_2
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  PyTorch Hub 
    ##                                                                                                                                             V137
    ## 1                                                                                                                                          Q21_3
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  Huggingface Models 
    ##                                                                                                                               V138
    ## 1                                                                                                                            Q21_4
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  Timm 
    ##                                                                                                                                    V139
    ## 1                                                                                                                                 Q21_5
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  Jumpstart 
    ##                                                                                                                                      V140
    ## 1                                                                                                                                   Q21_6
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  ONNX models 
    ##                                                                                                                                             V141
    ## 1                                                                                                                                          Q21_7
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  NVIDIA NGC models  
    ##                                                                                                                                          V142
    ## 1                                                                                                                                       Q21_8
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice -  Kaggle datasets 
    ##                                                                                                                                                                                           V143
    ## 1                                                                                                                                                                                        Q21_9
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice - No, I do not download pre-trained model weights on a regular basis
    ##                                                                                                                                                                   V144
    ## 1                                                                                                                                                               Q21_10
    ## 2 Do you download pre-trained model weights from any of the following services? (Select all that apply) - Selected Choice - Other storage services (i.e. google drive)
    ##                                                                                         V145
    ## 1                                                                                        Q22
    ## 2 Which of the following ML model hubs/repositories do you use most often? - Selected Choice
    ##                                                                                                      V146
    ## 1                                                                                                     Q23
    ## 2 Select the title most similar to your current role (or most recent title if retired): - Selected Choice
    ##                                                                                                              V147
    ## 1                                                                                                             Q24
    ## 2 In what industry is your current employer/contract (or your most recent employer if retired)? - Selected Choice
    ##                                                      V148
    ## 1                                                     Q25
    ## 2 What is the size of the company where you are employed?
    ##                                                                                                       V149
    ## 1                                                                                                      Q26
    ## 2 Approximately how many individuals are responsible for data science workloads at your place of business?
    ##                                                                                   V150
    ## 1                                                                                  Q27
    ## 2 Does your current employer incorporate machine learning methods into their business?
    ##                                                                                                                                                                          V151
    ## 1                                                                                                                                                                       Q28_1
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Analyze and understand data to influence product or business decisions
    ##                                                                                                                                                                                                                    V152
    ## 1                                                                                                                                                                                                                 Q28_2
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Build and/or run the data infrastructure that my business uses for storing, analyzing, and operationalizing data
    ##                                                                                                                                                                      V153
    ## 1                                                                                                                                                                   Q28_3
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Build prototypes to explore applying machine learning to new areas
    ##                                                                                                                                                                                                   V154
    ## 1                                                                                                                                                                                                Q28_4
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Build and/or run a machine learning service that operationally improves my product or workflows
    ##                                                                                                                                                               V155
    ## 1                                                                                                                                                            Q28_5
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Experimentation and iteration to improve existing ML models
    ##                                                                                                                                                                      V156
    ## 1                                                                                                                                                                   Q28_6
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Do research that advances the state of the art of machine learning
    ##                                                                                                                                                                     V157
    ## 1                                                                                                                                                                  Q28_7
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - None of these activities are an important part of my role at work
    ##                                                                                                         V158
    ## 1                                                                                                      Q28_8
    ## 2 Select any activities that make up an important part of your role at work: (Select all that apply) - Other
    ##                                                           V159
    ## 1                                                          Q29
    ## 2 What is your current yearly compensation (approximate $USD)?
    ##                                                                                                                                                                               V160
    ## 1                                                                                                                                                                              Q30
    ## 2 Approximately how much money have you spent on machine learning and/or cloud computing services at home or at work in the past 5 years (approximate $USD)?\n (approximate $USD)?
    ##                                                                                                                                   V161
    ## 1                                                                                                                                Q31_1
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Amazon Web Services (AWS) 
    ##                                                                                                                         V162
    ## 1                                                                                                                      Q31_2
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Microsoft Azure 
    ##                                                                                                                                     V163
    ## 1                                                                                                                                  Q31_3
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Google Cloud Platform (GCP) 
    ##                                                                                                                             V164
    ## 1                                                                                                                          Q31_4
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  IBM Cloud / Red Hat 
    ##                                                                                                                      V165
    ## 1                                                                                                                   Q31_5
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Oracle Cloud 
    ##                                                                                                                   V166
    ## 1                                                                                                                Q31_6
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  SAP Cloud 
    ##                                                                                                                      V167
    ## 1                                                                                                                   Q31_7
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  VMware Cloud 
    ##                                                                                                                       V168
    ## 1                                                                                                                    Q31_8
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Alibaba Cloud 
    ##                                                                                                                       V169
    ## 1                                                                                                                    Q31_9
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Tencent Cloud 
    ##                                                                                                                      V170
    ## 1                                                                                                                  Q31_10
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice -  Huawei Cloud 
    ##                                                                                                            V171
    ## 1                                                                                                        Q31_11
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice - None
    ##                                                                                                             V172
    ## 1                                                                                                         Q31_12
    ## 2 Which of the following cloud computing platforms do you use? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                    V173
    ## 1                                                                                                                                   Q32
    ## 2 Of the cloud platforms that you are familiar with, which has the best developer experience (most enjoyable to use)? - Selected Choice
    ##                                                                                                                                         V174
    ## 1                                                                                                                                      Q33_1
    ## 2 Do you use any of the following cloud computing products? (Select all that apply) - Selected Choice -  Amazon Elastic Compute Cloud (EC2) 
    ##                                                                                                                                       V175
    ## 1                                                                                                                                    Q33_2
    ## 2 Do you use any of the following cloud computing products? (Select all that apply) - Selected Choice -  Microsoft Azure Virtual Machines 
    ##                                                                                                                                  V176
    ## 1                                                                                                                               Q33_3
    ## 2 Do you use any of the following cloud computing products? (Select all that apply) - Selected Choice -  Google Cloud Compute Engine 
    ##                                                                                                              V177
    ## 1                                                                                                           Q33_4
    ## 2 Do you use any of the following cloud computing products? (Select all that apply) - Selected Choice - No / None
    ##                                                                                                          V178
    ## 1                                                                                                       Q33_5
    ## 2 Do you use any of the following cloud computing products? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                              V179
    ## 1                                                                                                                           Q34_1
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice - Microsoft Azure Blob Storage
    ##                                                                                                                         V180
    ## 1                                                                                                                      Q34_2
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice -  Microsoft Azure Files 
    ##                                                                                                                                       V181
    ## 1                                                                                                                                    Q34_3
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice -  Amazon Simple Storage Service (S3)  
    ##                                                                                                                                     V182
    ## 1                                                                                                                                  Q34_4
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice -  Amazon Elastic File System (EFS)  
    ##                                                                                                                               V183
    ## 1                                                                                                                            Q34_5
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice - Google Cloud Storage (GCS)   
    ##                                                                                                                          V184
    ## 1                                                                                                                       Q34_6
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice -  Google Cloud Filestore 
    ##                                                                                                           V185
    ## 1                                                                                                        Q34_7
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice - No / None
    ##                                                                                                       V186
    ## 1                                                                                                    Q34_8
    ## 2 Do you use any of the following data storage products? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                                V187
    ## 1                                                                                                                                                             Q35_1
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - MySQL 
    ##                                                                                                                                                                     V188
    ## 1                                                                                                                                                                  Q35_2
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - PostgreSQL 
    ##                                                                                                                                                                 V189
    ## 1                                                                                                                                                              Q35_3
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - SQLite 
    ##                                                                                                                                                                          V190
    ## 1                                                                                                                                                                       Q35_4
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Oracle Database 
    ##                                                                                                                                                                  V191
    ## 1                                                                                                                                                               Q35_5
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - MongoDB 
    ##                                                                                                                                                                    V192
    ## 1                                                                                                                                                                 Q35_6
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Snowflake 
    ##                                                                                                                                                                  V193
    ## 1                                                                                                                                                               Q35_7
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - IBM Db2 
    ##                                                                                                                                                                               V194
    ## 1                                                                                                                                                                            Q35_8
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Microsoft SQL Server 
    ##                                                                                                                                                                                       V195
    ## 1                                                                                                                                                                                    Q35_9
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Microsoft Azure SQL Database 
    ##                                                                                                                                                                          V196
    ## 1                                                                                                                                                                      Q35_10
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Amazon Redshift 
    ##                                                                                                                                                                     V197
    ## 1                                                                                                                                                                 Q35_11
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Amazon RDS 
    ##                                                                                                                                                                          V198
    ## 1                                                                                                                                                                      Q35_12
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Amazon DynamoDB 
    ##                                                                                                                                                                                V199
    ## 1                                                                                                                                                                            Q35_13
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Google Cloud BigQuery 
    ##                                                                                                                                                                           V200
    ## 1                                                                                                                                                                       Q35_14
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Google Cloud SQL 
    ##                                                                                                                                                              V201
    ## 1                                                                                                                                                          Q35_15
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - None
    ##                                                                                                                                                               V202
    ## 1                                                                                                                                                           Q35_16
    ## 2 Do you use any of the following data products (relational databases, data warehouses, data lakes, or similar)? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                         V203
    ## 1                                                                                                                      Q36_1
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Amazon QuickSight
    ##                                                                                                                          V204
    ## 1                                                                                                                       Q36_2
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Microsoft Power BI
    ##                                                                                                                          V205
    ## 1                                                                                                                       Q36_3
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Google Data Studio
    ##                                                                                                              V206
    ## 1                                                                                                           Q36_4
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Looker
    ##                                                                                                               V207
    ## 1                                                                                                            Q36_5
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Tableau
    ##                                                                                                                  V208
    ## 1                                                                                                               Q36_6
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Qlik Sense
    ##                                                                                                            V209
    ## 1                                                                                                         Q36_7
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Domo
    ##                                                                                                                      V210
    ## 1                                                                                                                   Q36_8
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - TIBCO Spotfire
    ##                                                                                                                V211
    ## 1                                                                                                             Q36_9
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Alteryx 
    ##                                                                                                                V212
    ## 1                                                                                                            Q36_10
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Sisense 
    ##                                                                                                                            V213
    ## 1                                                                                                                        Q36_11
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - SAP Analytics Cloud 
    ##                                                                                                                                V214
    ## 1                                                                                                                            Q36_12
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Microsoft Azure Synapse 
    ##                                                                                                                    V215
    ## 1                                                                                                                Q36_13
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Thoughtspot 
    ##                                                                                                            V216
    ## 1                                                                                                        Q36_14
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - None
    ##                                                                                                             V217
    ## 1                                                                                                         Q36_15
    ## 2 Do you use any of the following business intelligence tools? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                   V218
    ## 1                                                                                                                                                Q37_1
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Amazon SageMaker 
    ##                                                                                                                                                                V219
    ## 1                                                                                                                                                             Q37_2
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Azure Machine Learning Studio 
    ##                                                                                                                                                        V220
    ## 1                                                                                                                                                     Q37_3
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Google Cloud Vertex AI
    ##                                                                                                                                           V221
    ## 1                                                                                                                                        Q37_4
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  DataRobot
    ##                                                                                                                                            V222
    ## 1                                                                                                                                         Q37_5
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Databricks
    ##                                                                                                                                         V223
    ## 1                                                                                                                                      Q37_6
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Dataiku
    ##                                                                                                                                         V224
    ## 1                                                                                                                                      Q37_7
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Alteryx
    ##                                                                                                                                            V225
    ## 1                                                                                                                                         Q37_8
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Rapidminer
    ##                                                                                                                                       V226
    ## 1                                                                                                                                    Q37_9
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  C3.ai
    ##                                                                                                                                                  V227
    ## 1                                                                                                                                              Q37_10
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  Domino Data Lab 
    ##                                                                                                                                               V228
    ## 1                                                                                                                                           Q37_11
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice -  H2O AI Cloud 
    ##                                                                                                                                          V229
    ## 1                                                                                                                                      Q37_12
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice - No / None
    ##                                                                                                                                      V230
    ## 1                                                                                                                                  Q37_13
    ## 2 Do you use any of the following managed machine learning products on a regular basis? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                   V231
    ## 1                                                                                                                                Q38_1
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -  Google Cloud AutoML 
    ##                                                                                                                                  V232
    ## 1                                                                                                                               Q38_2
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -  H2O Driverless AI  
    ##                                                                                                                                 V233
    ## 1                                                                                                                              Q38_3
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -  Databricks AutoML 
    ##                                                                                                                                V234
    ## 1                                                                                                                             Q38_4
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -  DataRobot AutoML 
    ##                                                                                                                                           V235
    ## 1                                                                                                                                        Q38_5
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -   Amazon Sagemaker Autopilot 
    ##                                                                                                                                                 V236
    ## 1                                                                                                                                              Q38_6
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice -   Azure Automated Machine Learning 
    ##                                                                                                                       V237
    ## 1                                                                                                                    Q38_7
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice - No / None
    ##                                                                                                                   V238
    ## 1                                                                                                                Q38_8
    ## 2 Do you use any of the following automated machine learning tools?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                       V239
    ## 1                                                                                                                                                    Q39_1
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  TensorFlow Extended (TFX) 
    ##                                                                                                                                        V240
    ## 1                                                                                                                                     Q39_2
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  TorchServe 
    ##                                                                                                                                          V241
    ## 1                                                                                                                                       Q39_3
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  ONNX Runtime 
    ##                                                                                                                                                     V242
    ## 1                                                                                                                                                  Q39_4
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  Triton Inference Server 
    ##                                                                                                                                                   V243
    ## 1                                                                                                                                                Q39_5
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  OpenVINO Model Server 
    ##                                                                                                                                    V244
    ## 1                                                                                                                                 Q39_6
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  KServe 
    ##                                                                                                                                     V245
    ## 1                                                                                                                                  Q39_7
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  BentoML 
    ##                                                                                                                                                      V246
    ## 1                                                                                                                                                   Q39_8
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  Multi Model Server (MMS) 
    ##                                                                                                                                         V247
    ## 1                                                                                                                                      Q39_9
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  Seldon Core 
    ##                                                                                                                                    V248
    ## 1                                                                                                                                Q39_10
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice -  MLflow 
    ##                                                                                                                                V249
    ## 1                                                                                                                            Q39_11
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                 V250
    ## 1                                                                                                                             Q39_12
    ## 2 Do you use any of the following products to serve your machine learning models?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                             V251
    ## 1                                                                                                                                          Q40_1
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Neptune.ai 
    ##                                                                                                                                                   V252
    ## 1                                                                                                                                                Q40_2
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Weights & Biases 
    ##                                                                                                                                           V253
    ## 1                                                                                                                                        Q40_3
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Comet.ml 
    ##                                                                                                                                              V254
    ## 1                                                                                                                                           Q40_4
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  TensorBoard 
    ##                                                                                                                                           V255
    ## 1                                                                                                                                        Q40_5
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Guild.ai 
    ##                                                                                                                                          V256
    ## 1                                                                                                                                       Q40_6
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  ClearML 
    ##                                                                                                                                         V257
    ## 1                                                                                                                                      Q40_7
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  MLflow 
    ##                                                                                                                                         V258
    ## 1                                                                                                                                      Q40_8
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Aporia 
    ##                                                                                                                                               V259
    ## 1                                                                                                                                            Q40_9
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Evidently AI 
    ##                                                                                                                                        V260
    ## 1                                                                                                                                    Q40_10
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Arize 
    ##                                                                                                                                          V261
    ## 1                                                                                                                                      Q40_11
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  WhyLabs 
    ##                                                                                                                                          V262
    ## 1                                                                                                                                      Q40_12
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  Fiddler 
    ##                                                                                                                                      V263
    ## 1                                                                                                                                  Q40_13
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice -  DVC 
    ##                                                                                                                                          V264
    ## 1                                                                                                                                      Q40_14
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice - No / None
    ##                                                                                                                                      V265
    ## 1                                                                                                                                  Q40_15
    ## 2 Do you use any tools to help monitor your machine learning models and/or experiments? (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                                                                                          V266
    ## 1                                                                                                                                                                                                                       Q41_1
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  Google Responsible AI Toolkit (LIT, What-if, Fairness Indicator, etc) 
    ##                                                                                                                                                                                                                                 V267
    ## 1                                                                                                                                                                                                                              Q41_2
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  Microsoft Responsible AI Resources (Fairlearn, Counterfit, InterpretML, etc) 
    ##                                                                                                                                                                                                                              V268
    ## 1                                                                                                                                                                                                                           Q41_3
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  IBM AI Ethics tools (AI Fairness 360, Adversarial Robustness Toolbox, etc 
    ##                                                                                                                                                                                               V269
    ## 1                                                                                                                                                                                            Q41_4
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  Amazon AI Ethics Tools (Clarify, A2I, etc) 
    ##                                                                                                                                                                                         V270
    ## 1                                                                                                                                                                                      Q41_5
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  The LinkedIn Fairness Toolkit (LiFT) 
    ##                                                                                                                                                             V271
    ## 1                                                                                                                                                          Q41_6
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  Audit-AI 
    ##                                                                                                                                                             V272
    ## 1                                                                                                                                                          Q41_7
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice -  Aequitas 
    ##                                                                                                                                                       V273
    ## 1                                                                                                                                                    Q41_8
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                                        V274
    ## 1                                                                                                                                                     Q41_9
    ## 2 Do you use any of the following responsible or ethical AI products in your machine learning practices?  (Select all that apply) - Selected Choice - Other
    ##                                                                                                                                                       V275
    ## 1                                                                                                                                                    Q42_1
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  GPUs 
    ##                                                                                                                                                       V276
    ## 1                                                                                                                                                    Q42_2
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  TPUs 
    ##                                                                                                                                                       V277
    ## 1                                                                                                                                                    Q42_3
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  IPUs 
    ##                                                                                                                                                       V278
    ## 1                                                                                                                                                    Q42_4
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  RDUs 
    ##                                                                                                                                                       V279
    ## 1                                                                                                                                                    Q42_5
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  WSEs 
    ##                                                                                                                                                                 V280
    ## 1                                                                                                                                                              Q42_6
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  Trainium Chips 
    ##                                                                                                                                                                   V281
    ## 1                                                                                                                                                                Q42_7
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice -  Inferentia Chips 
    ##                                                                                                                                                     V282
    ## 1                                                                                                                                                  Q42_8
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice - None
    ##                                                                                                                                                      V283
    ## 1                                                                                                                                                   Q42_9
    ## 2 Do you use any of the following types of specialized hardware when training machine learning models?  (Select all that apply) - Selected Choice - Other
    ##                                                                         V284
    ## 1                                                                        Q43
    ## 2 Approximately how many times have you used a TPU (tensor processing unit)?
    ##                                                                                                                                                          V285
    ## 1                                                                                                                                                       Q44_1
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Twitter (data science influencers)
    ##                                                                                                                                                                                V286
    ## 1                                                                                                                                                                             Q44_2
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Email newsletters (Data Elixir, O'Reilly Data & AI, etc)
    ##                                                                                                                                                       V287
    ## 1                                                                                                                                                    Q44_3
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Reddit (r/machinelearning, etc)
    ##                                                                                                                                                       V288
    ## 1                                                                                                                                                    Q44_4
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Kaggle (notebooks, forums, etc)
    ##                                                                                                                                                                            V289
    ## 1                                                                                                                                                                         Q44_5
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Course Forums (forums.fast.ai, Coursera forums, etc)
    ##                                                                                                                                                                          V290
    ## 1                                                                                                                                                                       Q44_6
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - YouTube (Kaggle YouTube, Cloud AI Adventures, etc)
    ##                                                                                                                                                                                  V291
    ## 1                                                                                                                                                                               Q44_7
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Podcasts (Chai Time Data Science, O’Reilly Data Show, etc)
    ##                                                                                                                                                                           V292
    ## 1                                                                                                                                                                        Q44_8
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Blogs (Towards Data Science, Analytics Vidhya, etc)
    ##                                                                                                                                                                                                  V293
    ## 1                                                                                                                                                                                               Q44_9
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Journal Publications (peer-reviewed journals, conference proceedings, etc)
    ##                                                                                                                                                                    V294
    ## 1                                                                                                                                                                Q44_10
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Slack Communities (ods.ai, kagglenoobs, etc)
    ##                                                                                                                            V295
    ## 1                                                                                                                        Q44_11
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - None
    ##                                                                                                                             V296
    ## 1                                                                                                                         Q44_12
    ## 2 Who/what are your favorite media sources that report on data science topics? (Select all that apply) - Selected Choice - Other

    df_index <- transpose(df_index[1:2,])

    helpful_platforms <- df1 %>% select(contains("Q7_"))
    data.frame(x=unlist(helpful_platforms))

    ##                           x
    ## Q7_11                      
    ## Q7_12    University courses
    ## Q7_13                      
    ## Q7_14                      
    ## Q7_15    University courses
    ## Q7_16                      
    ## Q7_17                      
    ## Q7_18    University courses
    ## Q7_19                      
    ## Q7_110   University courses
    ## Q7_111                     
    ## Q7_112   University courses
    ## Q7_113                     
    ## Q7_114   University courses
    ## Q7_115                     
    ## Q7_116                     
    ## Q7_117   University courses
    ## Q7_118   University courses
    ## Q7_119   University courses
    ## Q7_120   University courses
    ## Q7_121                     
    ## Q7_122   University courses
    ## Q7_123                     
    ## Q7_124                     
    ## Q7_125                     
    ## Q7_126                     
    ## Q7_127                     
    ## Q7_128   University courses
    ## Q7_129   University courses
    ## Q7_130   University courses
    ## Q7_131   University courses
    ## Q7_132                     
    ## Q7_133                     
    ## Q7_134                     
    ## Q7_135                     
    ## Q7_136                     
    ## Q7_137                     
    ## Q7_138                     
    ## Q7_139   University courses
    ## Q7_140                     
    ## Q7_141   University courses
    ## Q7_142                     
    ## Q7_143                     
    ## Q7_144                     
    ## Q7_145                     
    ## Q7_146                     
    ## Q7_147                     
    ## Q7_148   University courses
    ## Q7_149                     
    ## Q7_150                     
    ## Q7_151                     
    ## Q7_152                     
    ## Q7_153                     
    ## Q7_154                     
    ## Q7_155                     
    ## Q7_156                     
    ## Q7_157                     
    ## Q7_158                     
    ## Q7_159                     
    ## Q7_160   University courses
    ## Q7_161   University courses
    ## Q7_162                     
    ## Q7_163   University courses
    ## Q7_164                     
    ## Q7_165   University courses
    ## Q7_166   University courses
    ## Q7_167                     
    ## Q7_168   University courses
    ## Q7_169                     
    ## Q7_170                     
    ## Q7_171   University courses
    ## Q7_172                     
    ## Q7_173   University courses
    ## Q7_174                     
    ## Q7_175                     
    ## Q7_176                     
    ## Q7_177   University courses
    ## Q7_178                     
    ## Q7_179                     
    ## Q7_180                     
    ## Q7_181                     
    ## Q7_182                     
    ## Q7_183                     
    ## Q7_184   University courses
    ## Q7_185                     
    ## Q7_186                     
    ## Q7_187   University courses
    ## Q7_188                     
    ## Q7_189                     
    ## Q7_190   University courses
    ## Q7_191                     
    ## Q7_192                     
    ## Q7_193                     
    ## Q7_194                     
    ## Q7_195                     
    ## Q7_196   University courses
    ## Q7_197                     
    ## Q7_198                     
    ## Q7_199   University courses
    ## Q7_1100  University courses
    ## Q7_1101                    
    ## Q7_1102                    
    ## Q7_1103                    
    ## Q7_1104                    
    ## Q7_1105                    
    ## Q7_1106  University courses
    ## Q7_1107  University courses
    ## Q7_1108                    
    ## Q7_1109  University courses
    ## Q7_1110                    
    ## Q7_1111  University courses
    ## Q7_1112  University courses
    ## Q7_1113                    
    ## Q7_1114                    
    ## Q7_1115                    
    ## Q7_1116                    
    ## Q7_1117  University courses
    ## Q7_1118                    
    ## Q7_1119                    
    ## Q7_1120                    
    ## Q7_1121                    
    ## Q7_1122                    
    ## Q7_1123  University courses
    ## Q7_1124                    
    ## Q7_1125  University courses
    ## Q7_1126                    
    ## Q7_1127                    
    ## Q7_1128                    
    ## Q7_1129                    
    ## Q7_1130  University courses
    ## Q7_1131                    
    ## Q7_1132  University courses
    ## Q7_1133                    
    ## Q7_1134  University courses
    ## Q7_1135                    
    ## Q7_1136                    
    ## Q7_1137                    
    ## Q7_1138  University courses
    ## Q7_1139                    
    ## Q7_1140                    
    ## Q7_1141  University courses
    ## Q7_1142                    
    ## Q7_1143                    
    ## Q7_1144                    
    ## Q7_1145                    
    ## Q7_1146  University courses
    ## Q7_1147                    
    ## Q7_1148                    
    ## Q7_1149  University courses
    ## Q7_1150                    
    ## Q7_1151                    
    ## Q7_1152                    
    ## Q7_1153                    
    ## Q7_1154  University courses
    ## Q7_1155                    
    ## Q7_1156  University courses
    ## Q7_1157                    
    ## Q7_1158  University courses
    ## Q7_1159                    
    ## Q7_1160                    
    ## Q7_1161                    
    ## Q7_1162                    
    ## Q7_1163                    
    ## Q7_1164                    
    ## Q7_1165                    
    ## Q7_1166                    
    ## Q7_1167  University courses
    ## Q7_1168  University courses
    ## Q7_1169  University courses
    ## Q7_1170                    
    ## Q7_1171                    
    ## Q7_1172                    
    ## Q7_1173                    
    ## Q7_1174                    
    ## Q7_1175                    
    ## Q7_1176                    
    ## Q7_1177                    
    ## Q7_1178                    
    ## Q7_1179                    
    ## Q7_1180                    
    ## Q7_1181  University courses
    ## Q7_1182                    
    ## Q7_1183  University courses
    ## Q7_1184                    
    ## Q7_1185  University courses
    ## Q7_1186                    
    ## Q7_1187                    
    ## Q7_1188  University courses
    ## Q7_1189  University courses
    ## Q7_1190                    
    ## Q7_1191  University courses
    ## Q7_1192                    
    ## Q7_1193                    
    ## Q7_1194                    
    ## Q7_1195                    
    ## Q7_1196  University courses
    ## Q7_1197                    
    ## Q7_1198                    
    ## Q7_1199  University courses
    ## Q7_1200                    
    ## Q7_1201                    
    ## Q7_1202                    
    ## Q7_1203                    
    ## Q7_1204                    
    ## Q7_1205                    
    ## Q7_1206                    
    ## Q7_1207                    
    ## Q7_1208                    
    ## Q7_1209  University courses
    ## Q7_1210  University courses
    ## Q7_1211                    
    ## Q7_1212                    
    ## Q7_1213                    
    ## Q7_1214                    
    ## Q7_1215  University courses
    ## Q7_1216                    
    ## Q7_1217                    
    ## Q7_1218                    
    ## Q7_1219                    
    ## Q7_1220                    
    ## Q7_1221                    
    ## Q7_1222                    
    ## Q7_1223                    
    ## Q7_1224                    
    ## Q7_1225                    
    ## Q7_1226                    
    ## Q7_1227  University courses
    ## Q7_1228                    
    ## Q7_1229  University courses
    ## Q7_1230                    
    ## Q7_1231                    
    ## Q7_1232  University courses
    ## Q7_1233  University courses
    ## Q7_1234  University courses
    ## Q7_1235                    
    ## Q7_1236                    
    ## Q7_1237                    
    ## Q7_1238                    
    ## Q7_1239                    
    ## Q7_1240  University courses
    ## Q7_1241                    
    ## Q7_1242                    
    ## Q7_1243  University courses
    ## Q7_1244                    
    ## Q7_1245                    
    ## Q7_1246                    
    ## Q7_1247                    
    ## Q7_1248  University courses
    ## Q7_1249  University courses
    ## Q7_1250                    
    ## Q7_1251                    
    ## Q7_1252                    
    ## Q7_1253                    
    ## Q7_1254                    
    ## Q7_1255                    
    ## Q7_1256                    
    ## Q7_1257  University courses
    ## Q7_1258                    
    ## Q7_1259                    
    ## Q7_1260                    
    ## Q7_1261                    
    ## Q7_1262  University courses
    ## Q7_1263                    
    ## Q7_1264                    
    ## Q7_1265                    
    ## Q7_1266                    
    ## Q7_1267  University courses
    ## Q7_1268                    
    ## Q7_1269                    
    ## Q7_1270  University courses
    ## Q7_1271  University courses
    ## Q7_1272                    
    ## Q7_1273                    
    ## Q7_1274                    
    ## Q7_1275                    
    ## Q7_1276                    
    ## Q7_1277  University courses
    ## Q7_1278                    
    ## Q7_1279  University courses
    ## Q7_1280  University courses
    ## Q7_1281                    
    ## Q7_1282                    
    ## Q7_1283  University courses
    ## Q7_1284                    
    ## Q7_1285                    
    ## Q7_1286                    
    ## Q7_1287                    
    ## Q7_1288                    
    ## Q7_1289                    
    ## Q7_1290                    
    ## Q7_1291                    
    ## Q7_1292                    
    ## Q7_1293                    
    ## Q7_1294  University courses
    ## Q7_1295                    
    ## Q7_1296                    
    ## Q7_1297                    
    ## Q7_1298                    
    ## Q7_1299                    
    ## Q7_1300                    
    ## Q7_1301  University courses
    ## Q7_1302                    
    ## Q7_1303  University courses
    ## Q7_1304                    
    ## Q7_1305                    
    ## Q7_1306                    
    ## Q7_1307                    
    ## Q7_1308  University courses
    ## Q7_1309                    
    ## Q7_1310                    
    ## Q7_1311                    
    ## Q7_1312                    
    ## Q7_1313                    
    ## Q7_1314                    
    ## Q7_1315  University courses
    ## Q7_1316  University courses
    ## Q7_1317                    
    ## Q7_1318                    
    ## Q7_1319                    
    ## Q7_1320                    
    ## Q7_1321                    
    ## Q7_1322                    
    ## Q7_1323  University courses
    ## Q7_1324                    
    ## Q7_1325                    
    ## Q7_1326  University courses
    ## Q7_1327                    
    ## Q7_1328                    
    ## Q7_1329                    
    ## Q7_1330                    
    ## Q7_1331                    
    ## Q7_1332                    
    ## Q7_1333                    
    ## Q7_1334                    
    ## Q7_1335                    
    ## Q7_1336                    
    ## Q7_1337                    
    ## Q7_1338                    
    ## Q7_1339  University courses
    ## Q7_1340  University courses
    ## Q7_1341  University courses
    ## Q7_1342  University courses
    ## Q7_1343                    
    ## Q7_1344  University courses
    ## Q7_1345  University courses
    ## Q7_1346                    
    ## Q7_1347                    
    ## Q7_1348                    
    ## Q7_1349                    
    ## Q7_1350                    
    ## Q7_1351                    
    ## Q7_1352                    
    ## Q7_1353                    
    ## Q7_1354                    
    ## Q7_1355                    
    ## Q7_1356  University courses
    ## Q7_1357                    
    ## Q7_1358                    
    ## Q7_1359                    
    ## Q7_1360                    
    ## Q7_1361                    
    ## Q7_1362  University courses
    ## Q7_1363                    
    ## Q7_1364                    
    ## Q7_1365                    
    ## Q7_1366                    
    ## Q7_1367                    
    ## Q7_1368                    
    ## Q7_1369                    
    ## Q7_1370  University courses
    ## Q7_1371                    
    ## Q7_1372                    
    ## Q7_1373                    
    ## Q7_1374                    
    ## Q7_1375                    
    ## Q7_1376                    
    ## Q7_1377                    
    ## Q7_1378                    
    ## Q7_1379  University courses
    ## Q7_1380                    
    ## Q7_1381  University courses
    ## Q7_1382  University courses
    ## Q7_1383                    
    ## Q7_1384                    
    ## Q7_1385                    
    ## Q7_1386  University courses
    ## Q7_1387  University courses
    ## Q7_1388  University courses
    ## Q7_1389                    
    ## Q7_1390                    
    ## Q7_1391                    
    ## Q7_1392                    
    ## Q7_1393                    
    ## Q7_1394                    
    ## Q7_1395  University courses
    ## Q7_1396                    
    ## Q7_1397  University courses
    ## Q7_1398  University courses
    ## Q7_1399  University courses
    ## Q7_1400  University courses
    ## Q7_1401                    
    ## Q7_1402                    
    ## Q7_1403                    
    ## Q7_1404  University courses
    ## Q7_1405                    
    ## Q7_1406                    
    ## Q7_1407  University courses
    ## Q7_1408                    
    ## Q7_1409                    
    ## Q7_1410                    
    ## Q7_1411  University courses
    ## Q7_1412  University courses
    ## Q7_1413                    
    ## Q7_1414  University courses
    ## Q7_1415                    
    ## Q7_1416                    
    ## Q7_1417                    
    ## Q7_1418                    
    ## Q7_1419  University courses
    ## Q7_1420                    
    ## Q7_1421  University courses
    ## Q7_1422                    
    ## Q7_1423                    
    ## Q7_1424                    
    ## Q7_1425  University courses
    ## Q7_1426  University courses
    ## Q7_1427                    
    ## Q7_1428                    
    ## Q7_1429                    
    ## Q7_1430                    
    ## Q7_1431                    
    ## Q7_1432                    
    ## Q7_1433                    
    ## Q7_1434  University courses
    ## Q7_1435                    
    ## Q7_1436                    
    ## Q7_1437  University courses
    ## Q7_1438                    
    ## Q7_1439                    
    ## Q7_1440                    
    ## Q7_1441                    
    ## Q7_1442                    
    ## Q7_1443  University courses
    ## Q7_1444  University courses
    ## Q7_1445                    
    ## Q7_1446                    
    ## Q7_1447                    
    ## Q7_1448  University courses
    ## Q7_1449                    
    ## Q7_1450                    
    ## Q7_1451                    
    ## Q7_1452                    
    ## Q7_1453                    
    ## Q7_1454                    
    ## Q7_1455                    
    ## Q7_1456  University courses
    ## Q7_1457                    
    ## Q7_1458                    
    ## Q7_1459  University courses
    ## Q7_1460                    
    ## Q7_1461                    
    ## Q7_1462                    
    ## Q7_1463                    
    ## Q7_1464                    
    ## Q7_1465                    
    ## Q7_1466                    
    ## Q7_1467                    
    ## Q7_1468                    
    ## Q7_1469                    
    ## Q7_1470                    
    ## Q7_1471                    
    ## Q7_1472  University courses
    ## Q7_1473  University courses
    ## Q7_1474  University courses
    ## Q7_1475                    
    ## Q7_1476                    
    ## Q7_1477                    
    ## Q7_1478                    
    ## Q7_1479                    
    ## Q7_1480                    
    ## Q7_1481  University courses
    ## Q7_1482  University courses
    ## Q7_1483                    
    ## Q7_1484  University courses
    ## Q7_1485                    
    ## Q7_1486                    
    ## Q7_1487                    
    ## Q7_1488                    
    ## Q7_1489                    
    ## Q7_1490                    
    ## Q7_1491                    
    ## Q7_1492                    
    ## Q7_1493                    
    ## Q7_1494                    
    ## Q7_1495                    
    ## Q7_1496                    
    ## Q7_1497                    
    ## Q7_1498  University courses
    ## Q7_1499                    
    ## Q7_1500                    
    ## Q7_1501                    
    ## Q7_1502                    
    ## Q7_1503                    
    ## Q7_1504                    
    ## Q7_1505                    
    ## Q7_1506  University courses
    ## Q7_1507                    
    ## Q7_1508  University courses
    ## Q7_1509                    
    ## Q7_1510                    
    ## Q7_1511                    
    ## Q7_1512                    
    ## Q7_1513                    
    ## Q7_1514  University courses
    ## Q7_1515  University courses
    ## Q7_1516                    
    ## Q7_1517                    
    ## Q7_1518  University courses
    ## Q7_1519  University courses
    ## Q7_1520                    
    ## Q7_1521                    
    ## Q7_1522  University courses
    ## Q7_1523  University courses
    ## Q7_1524                    
    ## Q7_1525                    
    ## Q7_1526                    
    ## Q7_1527                    
    ## Q7_1528                    
    ## Q7_1529                    
    ## Q7_1530                    
    ## Q7_1531                    
    ## Q7_1532                    
    ## Q7_1533                    
    ## Q7_1534                    
    ## Q7_1535  University courses
    ## Q7_1536                    
    ## Q7_1537                    
    ## Q7_1538                    
    ## Q7_1539  University courses
    ## Q7_1540  University courses
    ## Q7_1541  University courses
    ## Q7_1542                    
    ## Q7_1543                    
    ## Q7_1544  University courses
    ## Q7_1545                    
    ## Q7_1546                    
    ## Q7_1547  University courses
    ## Q7_1548                    
    ## Q7_1549                    
    ## Q7_1550                    
    ## Q7_1551                    
    ## Q7_1552                    
    ## Q7_1553                    
    ## Q7_1554                    
    ## Q7_1555  University courses
    ## Q7_1556                    
    ## Q7_1557                    
    ## Q7_1558                    
    ## Q7_1559                    
    ## Q7_1560                    
    ## Q7_1561                    
    ## Q7_1562                    
    ## Q7_1563                    
    ## Q7_1564                    
    ## Q7_1565                    
    ## Q7_1566  University courses
    ## Q7_1567                    
    ## Q7_1568                    
    ## Q7_1569                    
    ## Q7_1570  University courses
    ## Q7_1571  University courses
    ## Q7_1572                    
    ## Q7_1573  University courses
    ## Q7_1574                    
    ## Q7_1575                    
    ## Q7_1576  University courses
    ## Q7_1577  University courses
    ## Q7_1578                    
    ## Q7_1579                    
    ## Q7_1580  University courses
    ## Q7_1581                    
    ## Q7_1582                    
    ## Q7_1583                    
    ## Q7_1584                    
    ## Q7_1585                    
    ## Q7_1586  University courses
    ## Q7_1587                    
    ## Q7_1588                    
    ## Q7_1589                    
    ## Q7_1590                    
    ## Q7_1591  University courses
    ## Q7_1592  University courses
    ## Q7_1593  University courses
    ## Q7_1594                    
    ## Q7_1595                    
    ## Q7_1596                    
    ## Q7_1597  University courses
    ## Q7_1598  University courses
    ## Q7_1599                    
    ## Q7_1600                    
    ## Q7_1601                    
    ## Q7_1602                    
    ## Q7_1603  University courses
    ## Q7_1604                    
    ## Q7_1605                    
    ## Q7_1606                    
    ## Q7_1607                    
    ## Q7_1608                    
    ## Q7_1609  University courses
    ## Q7_1610                    
    ## Q7_1611                    
    ## Q7_1612                    
    ## Q7_1613                    
    ## Q7_1614  University courses
    ## Q7_1615  University courses
    ## Q7_1616  University courses
    ## Q7_1617                    
    ## Q7_1618                    
    ## Q7_1619                    
    ## Q7_1620  University courses
    ## Q7_1621                    
    ## Q7_1622  University courses
    ## Q7_1623  University courses
    ## Q7_1624                    
    ## Q7_1625                    
    ## Q7_1626                    
    ## Q7_1627                    
    ## Q7_1628                    
    ## Q7_1629  University courses
    ## Q7_1630                    
    ## Q7_1631  University courses
    ## Q7_1632                    
    ## Q7_1633  University courses
    ## Q7_1634                    
    ## Q7_1635                    
    ## Q7_1636  University courses
    ## Q7_1637                    
    ## Q7_1638  University courses
    ## Q7_1639  University courses
    ## Q7_1640                    
    ## Q7_1641                    
    ## Q7_1642                    
    ## Q7_1643                    
    ## Q7_1644                    
    ## Q7_1645                    
    ## Q7_1646                    
    ## Q7_1647                    
    ## Q7_1648                    
    ## Q7_1649                    
    ## Q7_1650  University courses
    ## Q7_1651  University courses
    ## Q7_1652                    
    ## Q7_1653                    
    ## Q7_1654                    
    ## Q7_1655                    
    ## Q7_1656  University courses
    ## Q7_1657                    
    ## Q7_1658  University courses
    ## Q7_1659  University courses
    ## Q7_1660                    
    ## Q7_1661  University courses
    ## Q7_1662                    
    ## Q7_1663                    
    ## Q7_1664                    
    ## Q7_1665                    
    ## Q7_1666                    
    ## Q7_1667                    
    ## Q7_1668                    
    ## Q7_1669                    
    ## Q7_1670                    
    ## Q7_1671                    
    ## Q7_1672                    
    ## Q7_1673                    
    ## Q7_1674                    
    ## Q7_1675                    
    ## Q7_1676                    
    ## Q7_1677                    
    ## Q7_1678                    
    ## Q7_1679  University courses
    ## Q7_1680  University courses
    ## Q7_1681  University courses
    ## Q7_1682                    
    ## Q7_1683                    
    ## Q7_1684                    
    ## Q7_1685                    
    ## Q7_1686  University courses
    ## Q7_1687                    
    ## Q7_1688                    
    ## Q7_1689                    
    ## Q7_1690                    
    ## Q7_1691  University courses
    ## Q7_1692                    
    ## Q7_1693  University courses
    ## Q7_1694                    
    ## Q7_1695                    
    ## Q7_1696                    
    ## Q7_1697                    
    ## Q7_1698  University courses
    ## Q7_1699                    
    ## Q7_1700                    
    ## Q7_1701                    
    ## Q7_1702                    
    ## Q7_1703  University courses
    ## Q7_1704                    
    ## Q7_1705                    
    ## Q7_1706  University courses
    ## Q7_1707  University courses
    ## Q7_1708                    
    ## Q7_1709                    
    ## Q7_1710                    
    ## Q7_1711                    
    ## Q7_1712                    
    ## Q7_1713                    
    ## Q7_1714                    
    ## Q7_1715  University courses
    ## Q7_1716                    
    ## Q7_1717  University courses
    ## Q7_1718  University courses
    ## Q7_1719  University courses
    ## Q7_1720                    
    ## Q7_1721                    
    ## Q7_1722  University courses
    ## Q7_1723                    
    ## Q7_1724  University courses
    ## Q7_1725  University courses
    ## Q7_1726                    
    ## Q7_1727                    
    ## Q7_1728                    
    ## Q7_1729                    
    ## Q7_1730                    
    ## Q7_1731                    
    ## Q7_1732                    
    ## Q7_1733                    
    ## Q7_1734                    
    ## Q7_1735                    
    ## Q7_1736  University courses
    ## Q7_1737                    
    ## Q7_1738                    
    ## Q7_1739                    
    ## Q7_1740                    
    ## Q7_1741                    
    ## Q7_1742  University courses
    ## Q7_1743                    
    ## Q7_1744                    
    ## Q7_1745                    
    ## Q7_1746                    
    ## Q7_1747                    
    ## Q7_1748                    
    ## Q7_1749                    
    ## Q7_1750                    
    ## Q7_1751                    
    ## Q7_1752                    
    ## Q7_1753  University courses
    ## Q7_1754                    
    ## Q7_1755                    
    ## Q7_1756  University courses
    ## Q7_1757                    
    ## Q7_1758  University courses
    ## Q7_1759  University courses
    ## Q7_1760  University courses
    ## Q7_1761                    
    ## Q7_1762                    
    ## Q7_1763                    
    ## Q7_1764                    
    ## Q7_1765  University courses
    ## Q7_1766                    
    ## Q7_1767                    
    ## Q7_1768  University courses
    ## Q7_1769                    
    ## Q7_1770  University courses
    ## Q7_1771                    
    ## Q7_1772  University courses
    ## Q7_1773  University courses
    ## Q7_1774                    
    ## Q7_1775  University courses
    ## Q7_1776                    
    ## Q7_1777                    
    ## Q7_1778                    
    ## Q7_1779                    
    ## Q7_1780                    
    ## Q7_1781  University courses
    ## Q7_1782                    
    ## Q7_1783                    
    ## Q7_1784                    
    ## Q7_1785                    
    ## Q7_1786  University courses
    ## Q7_1787                    
    ## Q7_1788  University courses
    ## Q7_1789                    
    ## Q7_1790                    
    ## Q7_1791                    
    ## Q7_1792  University courses
    ## Q7_1793  University courses
    ## Q7_1794                    
    ## Q7_1795                    
    ## Q7_1796                    
    ## Q7_1797                    
    ## Q7_1798                    
    ## Q7_1799  University courses
    ## Q7_1800  University courses
    ## Q7_1801                    
    ## Q7_1802                    
    ## Q7_1803                    
    ## Q7_1804  University courses
    ## Q7_1805  University courses
    ## Q7_1806                    
    ## Q7_1807  University courses
    ## Q7_1808  University courses
    ## Q7_1809                    
    ## Q7_1810  University courses
    ## Q7_1811                    
    ## Q7_1812                    
    ## Q7_1813                    
    ## Q7_1814  University courses
    ## Q7_1815                    
    ## Q7_1816                    
    ## Q7_1817                    
    ## Q7_1818  University courses
    ## Q7_1819  University courses
    ## Q7_1820                    
    ## Q7_1821  University courses
    ## Q7_1822                    
    ## Q7_1823  University courses
    ## Q7_1824                    
    ## Q7_1825                    
    ## Q7_1826                    
    ## Q7_1827                    
    ## Q7_1828                    
    ## Q7_1829                    
    ## Q7_1830                    
    ## Q7_1831  University courses
    ## Q7_1832  University courses
    ## Q7_1833                    
    ## Q7_1834                    
    ## Q7_1835                    
    ## Q7_1836                    
    ## Q7_1837                    
    ## Q7_1838                    
    ## Q7_1839                    
    ## Q7_1840                    
    ## Q7_1841                    
    ## Q7_1842  University courses
    ## Q7_1843                    
    ## Q7_1844  University courses
    ## Q7_1845                    
    ## Q7_1846                    
    ## Q7_1847                    
    ## Q7_1848                    
    ## Q7_1849                    
    ## Q7_1850                    
    ## Q7_1851  University courses
    ## Q7_1852  University courses
    ## Q7_1853                    
    ## Q7_1854                    
    ## Q7_1855                    
    ## Q7_1856  University courses
    ## Q7_1857                    
    ## Q7_1858  University courses
    ## Q7_1859  University courses
    ## Q7_1860                    
    ## Q7_1861                    
    ## Q7_1862  University courses
    ## Q7_1863                    
    ## Q7_1864  University courses
    ## Q7_1865                    
    ## Q7_1866                    
    ## Q7_1867                    
    ## Q7_1868  University courses
    ## Q7_1869  University courses
    ## Q7_1870                    
    ## Q7_1871                    
    ## Q7_1872                    
    ## Q7_1873                    
    ## Q7_1874                    
    ## Q7_1875                    
    ## Q7_1876                    
    ## Q7_1877                    
    ## Q7_1878                    
    ## Q7_1879                    
    ## Q7_1880                    
    ## Q7_1881  University courses
    ## Q7_1882                    
    ## Q7_1883  University courses
    ## Q7_1884  University courses
    ## Q7_1885                    
    ## Q7_1886  University courses
    ## Q7_1887  University courses
    ## Q7_1888                    
    ## Q7_1889  University courses
    ## Q7_1890                    
    ## Q7_1891                    
    ## Q7_1892                    
    ## Q7_1893  University courses
    ## Q7_1894                    
    ## Q7_1895                    
    ## Q7_1896                    
    ## Q7_1897                    
    ## Q7_1898                    
    ## Q7_1899                    
    ## Q7_1900                    
    ## Q7_1901                    
    ## Q7_1902                    
    ## Q7_1903                    
    ## Q7_1904                    
    ## Q7_1905  University courses
    ## Q7_1906                    
    ## Q7_1907                    
    ## Q7_1908                    
    ## Q7_1909                    
    ## Q7_1910                    
    ## Q7_1911  University courses
    ## Q7_1912                    
    ## Q7_1913                    
    ## Q7_1914                    
    ## Q7_1915  University courses
    ## Q7_1916                    
    ## Q7_1917                    
    ## Q7_1918                    
    ## Q7_1919  University courses
    ## Q7_1920                    
    ## Q7_1921                    
    ## Q7_1922  University courses
    ## Q7_1923                    
    ## Q7_1924                    
    ## Q7_1925                    
    ## Q7_1926  University courses
    ## Q7_1927                    
    ## Q7_1928                    
    ## Q7_1929                    
    ## Q7_1930                    
    ## Q7_1931  University courses
    ## Q7_1932  University courses
    ## Q7_1933  University courses
    ## Q7_1934                    
    ## Q7_1935  University courses
    ## Q7_1936                    
    ## Q7_1937  University courses
    ## Q7_1938                    
    ## Q7_1939                    
    ## Q7_1940                    
    ## Q7_1941                    
    ## Q7_1942  University courses
    ## Q7_1943                    
    ## Q7_1944                    
    ## Q7_1945                    
    ## Q7_1946                    
    ## Q7_1947                    
    ## Q7_1948                    
    ## Q7_1949  University courses
    ## Q7_1950  University courses
    ## Q7_1951                    
    ## Q7_1952                    
    ## Q7_1953                    
    ## Q7_1954  University courses
    ## Q7_1955  University courses
    ## Q7_1956                    
    ## Q7_1957                    
    ## Q7_1958                    
    ## Q7_1959                    
    ## Q7_1960                    
    ## Q7_1961                    
    ## Q7_1962                    
    ## Q7_1963                    
    ## Q7_1964                    
    ## Q7_1965                    
    ## Q7_1966  University courses
    ## Q7_1967                    
    ## Q7_1968                    
    ## Q7_1969                    
    ## Q7_1970                    
    ## Q7_1971                    
    ## Q7_1972  University courses
    ## Q7_1973                    
    ## Q7_1974                    
    ## Q7_1975                    
    ## Q7_1976                    
    ## Q7_1977                    
    ## Q7_1978  University courses
    ## Q7_1979                    
    ## Q7_1980                    
    ## Q7_1981  University courses
    ## Q7_1982  University courses
    ## Q7_1983                    
    ## Q7_1984  University courses
    ## Q7_1985  University courses
    ## Q7_1986                    
    ## Q7_1987  University courses
    ## Q7_1988                    
    ## Q7_1989  University courses
    ## Q7_1990                    
    ## Q7_1991  University courses
    ## Q7_1992                    
    ## Q7_1993                    
    ## Q7_1994                    
    ## Q7_1995                    
    ## Q7_1996                    
    ## Q7_1997                    
    ## Q7_1998                    
    ## Q7_1999                    
    ## Q7_11000                   
    ##  [ reached 'max' / getOption("max.print") -- omitted 166979 rows ]

    helpful_platforms <- helpful_platforms %>% mutate_all(na_if, "")
    helpful_platforms <- data.frame(data.frame(x=unlist(helpful_platforms)))

    data.frame(sort(table(helpful_platforms), decreasing = TRUE))

    ##                                               x  Freq
    ## 1           Online courses (Coursera, EdX, etc) 13714
    ## 2        Video platforms (YouTube, Twitch, etc) 12871
    ## 3         Kaggle (notebooks, competitions, etc) 12700
    ## 4                            University courses  6851
    ## 5 Social media platforms (Reddit, Twitter, etc)  3310
    ## 6                                         Other  1944
    ## 7            None / I do not study data science  1022

    data.frame(sort(table(df1[,173]), decreasing=TRUE))

    ##                                                       Var1  Freq
    ## 1                                                          22068
    ## 2                               Amazon Web Services (AWS)    555
    ## 3                             Google Cloud Platform (GCP)    501
    ## 4  They all had a similarly enjoyable developer experience   443
    ## 5                                         Microsoft Azure    256
    ## 6                                   None were satisfactory    72
    ## 7                                     IBM Cloud / Red Hat     34
    ## 8                                            Oracle Cloud     20
    ## 9                                                    Other    20
    ## 10                                           VMware Cloud     12
    ## 11                                              SAP Cloud      7
    ## 12                                          Alibaba Cloud      5
    ## 13                                          Tencent Cloud      3
    ## 14                                           Huawei Cloud      1

## Q3 + Q29

    gender_pay <- df1 %>% select(c("Q3","Q29"))
    #24,000 rows with NULLS

    gender_pay <- gender_pay %>% mutate_all(na_if, "")
    #clean NAs

    gender_pay %>% group_by(Q3) %>% count(Q29)

    ## # A tibble: 102 × 3
    ## # Groups:   Q3 [5]
    ##    Q3    Q29                  n
    ##    <chr> <chr>            <int>
    ##  1 Man   >$1,000,000         20
    ##  2 Man   $0-999             797
    ##  3 Man   $500,000-999,999    43
    ##  4 Man   1,000-1,999        354
    ##  5 Man   10,000-14,999      401
    ##  6 Man   100,000-124,999    339
    ##  7 Man   125,000-149,999    220
    ##  8 Man   15,000-19,999      241
    ##  9 Man   150,000-199,999    289
    ## 10 Man   2,000-2,999        216
    ## # … with 92 more rows

    #data.frame(table(gender_pay$Q29)) %>% arrange(Freq)
    gender_pay %>% count(Q3)

    ##                        Q3     n
    ## 1                     Man 18266
    ## 2               Nonbinary    78
    ## 3       Prefer not to say   334
    ## 4 Prefer to self-describe    33
    ## 5                   Woman  5286

    gender_paygap <- gender_pay %>% filter(Q3 == "Man" | Q3 == "Woman")
    gender_paygap %>% group_by(Q3) %>% count(Q29)

    ## # A tibble: 54 × 3
    ## # Groups:   Q3 [2]
    ##    Q3    Q29                  n
    ##    <chr> <chr>            <int>
    ##  1 Man   >$1,000,000         20
    ##  2 Man   $0-999             797
    ##  3 Man   $500,000-999,999    43
    ##  4 Man   1,000-1,999        354
    ##  5 Man   10,000-14,999      401
    ##  6 Man   100,000-124,999    339
    ##  7 Man   125,000-149,999    220
    ##  8 Man   15,000-19,999      241
    ##  9 Man   150,000-199,999    289
    ## 10 Man   2,000-2,999        216
    ## # … with 44 more rows

We are almost there, but the dollar signs are messing with the
hierarchies…

    gender_paygap %>% count(Q29)

    ##                 Q29     n
    ## 1       >$1,000,000    22
    ## 2            $0-999  1093
    ## 3  $500,000-999,999    48
    ## 4       1,000-1,999   439
    ## 5     10,000-14,999   487
    ## 6   100,000-124,999   395
    ## 7   125,000-149,999   265
    ## 8     15,000-19,999   297
    ## 9   150,000-199,999   331
    ## 10      2,000-2,999   268
    ## 11    20,000-24,999   336
    ## 12  200,000-249,999   150
    ## 13    25,000-29,999   273
    ## 14  250,000-299,999    77
    ## 15      3,000-3,999   242
    ## 16    30,000-39,999   457
    ## 17  300,000-499,999    72
    ## 18      4,000-4,999   231
    ## 19    40,000-49,999   413
    ## 20      5,000-7,499   388
    ## 21    50,000-59,999   358
    ## 22    60,000-69,999   316
    ## 23      7,500-9,999   360
    ## 24    70,000-79,999   287
    ## 25    80,000-89,999   219
    ## 26    90,000-99,999   193
    ## 27             <NA> 15535

    gender_paygap %>% mutate(across(starts_with("Q29"), ~gsub("\\$", "", .))) %>% count(Q29) %>% arrange(Q29)

    ##                Q29     n
    ## 1       >1,000,000    22
    ## 2            0-999  1093
    ## 3      1,000-1,999   439
    ## 4    10,000-14,999   487
    ## 5  100,000-124,999   395
    ## 6  125,000-149,999   265
    ## 7    15,000-19,999   297
    ## 8  150,000-199,999   331
    ## 9      2,000-2,999   268
    ## 10   20,000-24,999   336
    ## 11 200,000-249,999   150
    ## 12   25,000-29,999   273
    ## 13 250,000-299,999    77
    ## 14     3,000-3,999   242
    ## 15   30,000-39,999   457
    ## 16 300,000-499,999    72
    ## 17     4,000-4,999   231
    ## 18   40,000-49,999   413
    ## 19     5,000-7,499   388
    ## 20   50,000-59,999   358
    ## 21 500,000-999,999    48
    ## 22   60,000-69,999   316
    ## 23     7,500-9,999   360
    ## 24   70,000-79,999   287
    ## 25   80,000-89,999   219
    ## 26   90,000-99,999   193
    ## 27            <NA> 15535

    gender_paygap %>% mutate(across(starts_with("Q29"), ~gsub("\\$", "", .))) %>% mutate(across(starts_with("Q29"), ~gsub("\\,", "", .))) %>% count(Q29) %>% arrange(Q29)

    ##              Q29     n
    ## 1       >1000000    22
    ## 2          0-999  1093
    ## 3      1000-1999   439
    ## 4    10000-14999   487
    ## 5  100000-124999   395
    ## 6  125000-149999   265
    ## 7    15000-19999   297
    ## 8  150000-199999   331
    ## 9      2000-2999   268
    ## 10   20000-24999   336
    ## 11 200000-249999   150
    ## 12   25000-29999   273
    ## 13 250000-299999    77
    ## 14     3000-3999   242
    ## 15   30000-39999   457
    ## 16 300000-499999    72
    ## 17     4000-4999   231
    ## 18   40000-49999   413
    ## 19     5000-7499   388
    ## 20   50000-59999   358
    ## 21 500000-999999    48
    ## 22   60000-69999   316
    ## 23   70000-79999   287
    ## 24     7500-9999   360
    ## 25   80000-89999   219
    ## 26   90000-99999   193
    ## 27          <NA> 15535

    gender_paygap <- gender_paygap %>% mutate(across(starts_with("Q29"), ~gsub("\\$", "", .))) %>% mutate(across(starts_with("Q29"), ~gsub("\\,", "", .)))
    # stripping out "$" and "commas"

    gender_paygap_range <- gender_paygap %>% separate(Q29, c("MINsal", "MAXsal"))
    #string split by "-"

    millionaires <- gender_paygap %>% filter(Q29 == ">1000000")
    #as.numeric(gender_paygap_range$MINsal)

    as.numeric(millionaires$Q29)

    ## Warning: NAs introduced by coercion

    ##  [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA

    #million+ earners would get NA-coerced by data type transformation

    gender_paygap <- gender_paygap %>% filter(Q29 != ">1000000")
    #remove millionaires

    gender_paygap <- gender_paygap %>% separate(Q29, c("MINsal", "MAXsal"))
    #string split
    gender_paygap$MINsal <- as.numeric(gender_paygap$MINsal)
    gender_paygap$MAXsal <- as.numeric(gender_paygap$MAXsal)
    #convert to numeric data

    salary_data <- data.frame(gender_paygap)
    salary_data

    ##        Q3 MINsal MAXsal
    ## 1     Man  25000  29999
    ## 2     Man 100000 124999
    ## 3     Man 100000 124999
    ## 4     Man 200000 249999
    ## 5     Man 200000 249999
    ## 6     Man 150000 199999
    ## 7     Man  90000  99999
    ## 8     Man  30000  39999
    ## 9     Man  30000  39999
    ## 10    Man  30000  39999
    ## 11  Woman   3000   3999
    ## 12    Man  50000  59999
    ## 13    Man 125000 149999
    ## 14    Man  15000  19999
    ## 15    Man  50000  59999
    ## 16  Woman   5000   7499
    ## 17    Man  10000  14999
    ## 18    Man  30000  39999
    ## 19    Man  20000  24999
    ## 20    Man   3000   3999
    ## 21    Man  25000  29999
    ## 22    Man      0    999
    ## 23    Man  50000  59999
    ## 24    Man   3000   3999
    ## 25    Man   7500   9999
    ## 26    Man   4000   4999
    ## 27    Man 100000 124999
    ## 28  Woman  25000  29999
    ## 29    Man      0    999
    ## 30    Man  80000  89999
    ## 31    Man  20000  24999
    ## 32    Man  15000  19999
    ## 33    Man   3000   3999
    ## 34    Man      0    999
    ## 35  Woman  90000  99999
    ## 36    Man      0    999
    ## 37    Man   4000   4999
    ## 38    Man   3000   3999
    ## 39    Man 125000 149999
    ## 40    Man  50000  59999
    ## 41    Man  30000  39999
    ## 42    Man  30000  39999
    ## 43    Man 150000 199999
    ## 44    Man   3000   3999
    ## 45    Man   7500   9999
    ## 46  Woman  50000  59999
    ## 47  Woman      0    999
    ## 48  Woman      0    999
    ## 49    Man  20000  24999
    ## 50    Man   4000   4999
    ## 51    Man      0    999
    ## 52    Man 100000 124999
    ## 53    Man   3000   3999
    ## 54    Man   2000   2999
    ## 55  Woman      0    999
    ## 56    Man      0    999
    ## 57    Man  80000  89999
    ## 58    Man  30000  39999
    ## 59    Man 250000 299999
    ## 60    Man  20000  24999
    ## 61    Man      0    999
    ## 62    Man   3000   3999
    ## 63    Man  80000  89999
    ## 64    Man  90000  99999
    ## 65    Man 150000 199999
    ## 66    Man  30000  39999
    ## 67    Man   4000   4999
    ## 68    Man 125000 149999
    ## 69  Woman   1000   1999
    ## 70    Man   3000   3999
    ## 71    Man 500000 999999
    ## 72    Man 150000 199999
    ## 73  Woman   7500   9999
    ## 74    Man  70000  79999
    ## 75    Man   3000   3999
    ## 76  Woman   7500   9999
    ## 77  Woman      0    999
    ## 78    Man  10000  14999
    ## 79  Woman  10000  14999
    ## 80    Man      0    999
    ## 81    Man   7500   9999
    ## 82    Man  30000  39999
    ## 83    Man  10000  14999
    ## 84    Man  15000  19999
    ## 85    Man 100000 124999
    ## 86  Woman      0    999
    ## 87    Man   5000   7499
    ## 88    Man  50000  59999
    ## 89    Man 100000 124999
    ## 90    Man  10000  14999
    ## 91    Man   7500   9999
    ## 92  Woman   1000   1999
    ## 93    Man  50000  59999
    ## 94    Man      0    999
    ## 95    Man  30000  39999
    ## 96    Man  20000  24999
    ## 97    Man  60000  69999
    ## 98    Man  40000  49999
    ## 99    Man   1000   1999
    ## 100   Man   5000   7499
    ## 101 Woman      0    999
    ## 102 Woman      0    999
    ## 103 Woman 100000 124999
    ## 104   Man      0    999
    ## 105   Man  50000  59999
    ## 106   Man 125000 149999
    ## 107   Man  50000  59999
    ## 108   Man   3000   3999
    ## 109   Man 150000 199999
    ## 110   Man 150000 199999
    ## 111   Man  30000  39999
    ## 112   Man  90000  99999
    ## 113   Man  20000  24999
    ## 114   Man  40000  49999
    ## 115   Man   5000   7499
    ## 116   Man  30000  39999
    ## 117   Man 100000 124999
    ## 118   Man  25000  29999
    ## 119   Man 125000 149999
    ## 120   Man   5000   7499
    ## 121   Man  10000  14999
    ## 122   Man   1000   1999
    ## 123 Woman  15000  19999
    ## 124   Man   5000   7499
    ## 125 Woman  15000  19999
    ## 126   Man  15000  19999
    ## 127   Man  15000  19999
    ## 128 Woman   2000   2999
    ## 129 Woman  60000  69999
    ## 130   Man  90000  99999
    ## 131   Man  30000  39999
    ## 132   Man   1000   1999
    ## 133   Man   7500   9999
    ## 134   Man   4000   4999
    ## 135   Man      0    999
    ## 136   Man  40000  49999
    ## 137   Man  20000  24999
    ## 138 Woman  40000  49999
    ## 139   Man  60000  69999
    ## 140 Woman   5000   7499
    ## 141   Man  25000  29999
    ## 142 Woman      0    999
    ## 143 Woman  80000  89999
    ## 144 Woman  50000  59999
    ## 145   Man  40000  49999
    ## 146   Man      0    999
    ## 147   Man   4000   4999
    ## 148   Man   2000   2999
    ## 149   Man 125000 149999
    ## 150   Man   2000   2999
    ## 151   Man      0    999
    ## 152 Woman   4000   4999
    ## 153   Man  40000  49999
    ## 154   Man   7500   9999
    ## 155   Man   1000   1999
    ## 156   Man   7500   9999
    ## 157   Man  80000  89999
    ## 158   Man   5000   7499
    ## 159   Man  25000  29999
    ## 160   Man  30000  39999
    ## 161   Man  25000  29999
    ## 162   Man   1000   1999
    ## 163   Man  15000  19999
    ## 164   Man  70000  79999
    ## 165   Man  70000  79999
    ## 166 Woman  25000  29999
    ## 167   Man  25000  29999
    ## 168   Man      0    999
    ## 169   Man      0    999
    ## 170   Man  20000  24999
    ## 171   Man 200000 249999
    ## 172   Man 125000 149999
    ## 173   Man   3000   3999
    ## 174   Man  20000  24999
    ## 175 Woman  40000  49999
    ## 176   Man  20000  24999
    ## 177   Man 125000 149999
    ## 178   Man  80000  89999
    ## 179   Man   5000   7499
    ## 180   Man 125000 149999
    ## 181 Woman      0    999
    ## 182   Man 125000 149999
    ## 183   Man 100000 124999
    ## 184   Man  50000  59999
    ## 185 Woman 100000 124999
    ## 186 Woman      0    999
    ## 187   Man   2000   2999
    ## 188   Man 250000 299999
    ## 189   Man      0    999
    ## 190   Man  40000  49999
    ## 191   Man   1000   1999
    ## 192   Man  50000  59999
    ## 193   Man   3000   3999
    ## 194   Man      0    999
    ## 195   Man      0    999
    ## 196   Man   4000   4999
    ## 197   Man      0    999
    ## 198   Man  90000  99999
    ## 199   Man  40000  49999
    ## 200   Man      0    999
    ## 201 Woman 100000 124999
    ## 202   Man  40000  49999
    ## 203   Man  40000  49999
    ## 204 Woman  70000  79999
    ## 205   Man  70000  79999
    ## 206   Man  70000  79999
    ## 207   Man  40000  49999
    ## 208   Man 100000 124999
    ## 209   Man  10000  14999
    ## 210 Woman   3000   3999
    ## 211   Man   1000   1999
    ## 212   Man   3000   3999
    ## 213   Man  30000  39999
    ## 214   Man  15000  19999
    ## 215   Man  80000  89999
    ## 216 Woman  50000  59999
    ## 217   Man 200000 249999
    ## 218   Man   2000   2999
    ## 219 Woman      0    999
    ## 220   Man   1000   1999
    ## 221 Woman  80000  89999
    ## 222 Woman   7500   9999
    ## 223   Man   7500   9999
    ## 224 Woman 150000 199999
    ## 225   Man   3000   3999
    ## 226   Man  50000  59999
    ## 227   Man   3000   3999
    ## 228   Man  25000  29999
    ## 229   Man  10000  14999
    ## 230   Man  25000  29999
    ## 231   Man  70000  79999
    ## 232   Man   3000   3999
    ## 233   Man   5000   7499
    ## 234 Woman   1000   1999
    ## 235   Man  70000  79999
    ## 236   Man  30000  39999
    ## 237   Man 125000 149999
    ## 238   Man  90000  99999
    ## 239   Man   5000   7499
    ## 240   Man  10000  14999
    ## 241   Man  50000  59999
    ## 242 Woman   1000   1999
    ## 243   Man 150000 199999
    ## 244   Man      0    999
    ## 245   Man      0    999
    ## 246   Man 200000 249999
    ## 247   Man      0    999
    ## 248   Man   7500   9999
    ## 249   Man   2000   2999
    ## 250   Man  80000  89999
    ## 251   Man 100000 124999
    ## 252 Woman  60000  69999
    ## 253   Man      0    999
    ## 254   Man   5000   7499
    ## 255   Man  25000  29999
    ## 256   Man   7500   9999
    ## 257   Man   2000   2999
    ## 258   Man   1000   1999
    ## 259   Man   5000   7499
    ## 260   Man  60000  69999
    ## 261   Man 100000 124999
    ## 262   Man  30000  39999
    ## 263   Man 150000 199999
    ## 264   Man   2000   2999
    ## 265   Man      0    999
    ## 266   Man  10000  14999
    ## 267   Man  40000  49999
    ## 268   Man 200000 249999
    ## 269   Man 200000 249999
    ## 270   Man  80000  89999
    ## 271   Man   3000   3999
    ## 272   Man 100000 124999
    ## 273   Man  80000  89999
    ## 274   Man  80000  89999
    ## 275   Man      0    999
    ## 276 Woman  15000  19999
    ## 277   Man   2000   2999
    ## 278   Man  80000  89999
    ## 279   Man  90000  99999
    ## 280   Man  50000  59999
    ## 281   Man  25000  29999
    ## 282 Woman  25000  29999
    ## 283   Man  10000  14999
    ## 284   Man 100000 124999
    ## 285   Man  25000  29999
    ## 286   Man  10000  14999
    ## 287   Man      0    999
    ## 288   Man  30000  39999
    ## 289   Man  20000  24999
    ## 290   Man      0    999
    ## 291 Woman  10000  14999
    ## 292   Man   7500   9999
    ## 293   Man  70000  79999
    ## 294 Woman      0    999
    ## 295   Man      0    999
    ## 296 Woman   5000   7499
    ## 297   Man  70000  79999
    ## 298   Man 100000 124999
    ## 299   Man   7500   9999
    ## 300   Man   4000   4999
    ## 301 Woman  80000  89999
    ## 302 Woman   7500   9999
    ## 303 Woman  20000  24999
    ## 304   Man  40000  49999
    ## 305 Woman      0    999
    ## 306   Man 500000 999999
    ## 307   Man   7500   9999
    ## 308 Woman  25000  29999
    ## 309   Man  80000  89999
    ## 310   Man      0    999
    ## 311 Woman  70000  79999
    ## 312 Woman   7500   9999
    ## 313   Man 150000 199999
    ## 314   Man      0    999
    ## 315   Man  90000  99999
    ## 316   Man  90000  99999
    ## 317   Man  25000  29999
    ## 318   Man  30000  39999
    ## 319   Man   5000   7499
    ## 320   Man  40000  49999
    ## 321   Man 100000 124999
    ## 322 Woman      0    999
    ## 323   Man  30000  39999
    ## 324   Man   4000   4999
    ## 325   Man  50000  59999
    ## 326   Man   5000   7499
    ## 327   Man   5000   7499
    ## 328 Woman      0    999
    ## 329   Man      0    999
    ## 330   Man      0    999
    ## 331 Woman 200000 249999
    ## 332   Man  20000  24999
    ## 333   Man 125000 149999
    ##  [ reached 'max' / getOption("max.print") -- omitted 7662 rows ]

    gender_paygap %>% 
      mutate(
        salary_range = paste0 (
      format(MINsal, trim = TRUE),
      "-", 
      MAXsal
    ),
    salary_range = fct_reorder(salary_range, MINsal)
      )

    ##        Q3 MINsal MAXsal  salary_range
    ## 1     Man  25000  29999   25000-29999
    ## 2     Man 100000 124999 100000-124999
    ## 3     Man 100000 124999 100000-124999
    ## 4     Man 200000 249999 200000-249999
    ## 5     Man 200000 249999 200000-249999
    ## 6     Man 150000 199999 150000-199999
    ## 7     Man  90000  99999   90000-99999
    ## 8     Man  30000  39999   30000-39999
    ## 9     Man  30000  39999   30000-39999
    ## 10    Man  30000  39999   30000-39999
    ## 11  Woman   3000   3999     3000-3999
    ## 12    Man  50000  59999   50000-59999
    ## 13    Man 125000 149999 125000-149999
    ## 14    Man  15000  19999   15000-19999
    ## 15    Man  50000  59999   50000-59999
    ## 16  Woman   5000   7499     5000-7499
    ## 17    Man  10000  14999   10000-14999
    ## 18    Man  30000  39999   30000-39999
    ## 19    Man  20000  24999   20000-24999
    ## 20    Man   3000   3999     3000-3999
    ## 21    Man  25000  29999   25000-29999
    ## 22    Man      0    999         0-999
    ## 23    Man  50000  59999   50000-59999
    ## 24    Man   3000   3999     3000-3999
    ## 25    Man   7500   9999     7500-9999
    ## 26    Man   4000   4999     4000-4999
    ## 27    Man 100000 124999 100000-124999
    ## 28  Woman  25000  29999   25000-29999
    ## 29    Man      0    999         0-999
    ## 30    Man  80000  89999   80000-89999
    ## 31    Man  20000  24999   20000-24999
    ## 32    Man  15000  19999   15000-19999
    ## 33    Man   3000   3999     3000-3999
    ## 34    Man      0    999         0-999
    ## 35  Woman  90000  99999   90000-99999
    ## 36    Man      0    999         0-999
    ## 37    Man   4000   4999     4000-4999
    ## 38    Man   3000   3999     3000-3999
    ## 39    Man 125000 149999 125000-149999
    ## 40    Man  50000  59999   50000-59999
    ## 41    Man  30000  39999   30000-39999
    ## 42    Man  30000  39999   30000-39999
    ## 43    Man 150000 199999 150000-199999
    ## 44    Man   3000   3999     3000-3999
    ## 45    Man   7500   9999     7500-9999
    ## 46  Woman  50000  59999   50000-59999
    ## 47  Woman      0    999         0-999
    ## 48  Woman      0    999         0-999
    ## 49    Man  20000  24999   20000-24999
    ## 50    Man   4000   4999     4000-4999
    ## 51    Man      0    999         0-999
    ## 52    Man 100000 124999 100000-124999
    ## 53    Man   3000   3999     3000-3999
    ## 54    Man   2000   2999     2000-2999
    ## 55  Woman      0    999         0-999
    ## 56    Man      0    999         0-999
    ## 57    Man  80000  89999   80000-89999
    ## 58    Man  30000  39999   30000-39999
    ## 59    Man 250000 299999 250000-299999
    ## 60    Man  20000  24999   20000-24999
    ## 61    Man      0    999         0-999
    ## 62    Man   3000   3999     3000-3999
    ## 63    Man  80000  89999   80000-89999
    ## 64    Man  90000  99999   90000-99999
    ## 65    Man 150000 199999 150000-199999
    ## 66    Man  30000  39999   30000-39999
    ## 67    Man   4000   4999     4000-4999
    ## 68    Man 125000 149999 125000-149999
    ## 69  Woman   1000   1999     1000-1999
    ## 70    Man   3000   3999     3000-3999
    ## 71    Man 500000 999999 500000-999999
    ## 72    Man 150000 199999 150000-199999
    ## 73  Woman   7500   9999     7500-9999
    ## 74    Man  70000  79999   70000-79999
    ## 75    Man   3000   3999     3000-3999
    ## 76  Woman   7500   9999     7500-9999
    ## 77  Woman      0    999         0-999
    ## 78    Man  10000  14999   10000-14999
    ## 79  Woman  10000  14999   10000-14999
    ## 80    Man      0    999         0-999
    ## 81    Man   7500   9999     7500-9999
    ## 82    Man  30000  39999   30000-39999
    ## 83    Man  10000  14999   10000-14999
    ## 84    Man  15000  19999   15000-19999
    ## 85    Man 100000 124999 100000-124999
    ## 86  Woman      0    999         0-999
    ## 87    Man   5000   7499     5000-7499
    ## 88    Man  50000  59999   50000-59999
    ## 89    Man 100000 124999 100000-124999
    ## 90    Man  10000  14999   10000-14999
    ## 91    Man   7500   9999     7500-9999
    ## 92  Woman   1000   1999     1000-1999
    ## 93    Man  50000  59999   50000-59999
    ## 94    Man      0    999         0-999
    ## 95    Man  30000  39999   30000-39999
    ## 96    Man  20000  24999   20000-24999
    ## 97    Man  60000  69999   60000-69999
    ## 98    Man  40000  49999   40000-49999
    ## 99    Man   1000   1999     1000-1999
    ## 100   Man   5000   7499     5000-7499
    ## 101 Woman      0    999         0-999
    ## 102 Woman      0    999         0-999
    ## 103 Woman 100000 124999 100000-124999
    ## 104   Man      0    999         0-999
    ## 105   Man  50000  59999   50000-59999
    ## 106   Man 125000 149999 125000-149999
    ## 107   Man  50000  59999   50000-59999
    ## 108   Man   3000   3999     3000-3999
    ## 109   Man 150000 199999 150000-199999
    ## 110   Man 150000 199999 150000-199999
    ## 111   Man  30000  39999   30000-39999
    ## 112   Man  90000  99999   90000-99999
    ## 113   Man  20000  24999   20000-24999
    ## 114   Man  40000  49999   40000-49999
    ## 115   Man   5000   7499     5000-7499
    ## 116   Man  30000  39999   30000-39999
    ## 117   Man 100000 124999 100000-124999
    ## 118   Man  25000  29999   25000-29999
    ## 119   Man 125000 149999 125000-149999
    ## 120   Man   5000   7499     5000-7499
    ## 121   Man  10000  14999   10000-14999
    ## 122   Man   1000   1999     1000-1999
    ## 123 Woman  15000  19999   15000-19999
    ## 124   Man   5000   7499     5000-7499
    ## 125 Woman  15000  19999   15000-19999
    ## 126   Man  15000  19999   15000-19999
    ## 127   Man  15000  19999   15000-19999
    ## 128 Woman   2000   2999     2000-2999
    ## 129 Woman  60000  69999   60000-69999
    ## 130   Man  90000  99999   90000-99999
    ## 131   Man  30000  39999   30000-39999
    ## 132   Man   1000   1999     1000-1999
    ## 133   Man   7500   9999     7500-9999
    ## 134   Man   4000   4999     4000-4999
    ## 135   Man      0    999         0-999
    ## 136   Man  40000  49999   40000-49999
    ## 137   Man  20000  24999   20000-24999
    ## 138 Woman  40000  49999   40000-49999
    ## 139   Man  60000  69999   60000-69999
    ## 140 Woman   5000   7499     5000-7499
    ## 141   Man  25000  29999   25000-29999
    ## 142 Woman      0    999         0-999
    ## 143 Woman  80000  89999   80000-89999
    ## 144 Woman  50000  59999   50000-59999
    ## 145   Man  40000  49999   40000-49999
    ## 146   Man      0    999         0-999
    ## 147   Man   4000   4999     4000-4999
    ## 148   Man   2000   2999     2000-2999
    ## 149   Man 125000 149999 125000-149999
    ## 150   Man   2000   2999     2000-2999
    ## 151   Man      0    999         0-999
    ## 152 Woman   4000   4999     4000-4999
    ## 153   Man  40000  49999   40000-49999
    ## 154   Man   7500   9999     7500-9999
    ## 155   Man   1000   1999     1000-1999
    ## 156   Man   7500   9999     7500-9999
    ## 157   Man  80000  89999   80000-89999
    ## 158   Man   5000   7499     5000-7499
    ## 159   Man  25000  29999   25000-29999
    ## 160   Man  30000  39999   30000-39999
    ## 161   Man  25000  29999   25000-29999
    ## 162   Man   1000   1999     1000-1999
    ## 163   Man  15000  19999   15000-19999
    ## 164   Man  70000  79999   70000-79999
    ## 165   Man  70000  79999   70000-79999
    ## 166 Woman  25000  29999   25000-29999
    ## 167   Man  25000  29999   25000-29999
    ## 168   Man      0    999         0-999
    ## 169   Man      0    999         0-999
    ## 170   Man  20000  24999   20000-24999
    ## 171   Man 200000 249999 200000-249999
    ## 172   Man 125000 149999 125000-149999
    ## 173   Man   3000   3999     3000-3999
    ## 174   Man  20000  24999   20000-24999
    ## 175 Woman  40000  49999   40000-49999
    ## 176   Man  20000  24999   20000-24999
    ## 177   Man 125000 149999 125000-149999
    ## 178   Man  80000  89999   80000-89999
    ## 179   Man   5000   7499     5000-7499
    ## 180   Man 125000 149999 125000-149999
    ## 181 Woman      0    999         0-999
    ## 182   Man 125000 149999 125000-149999
    ## 183   Man 100000 124999 100000-124999
    ## 184   Man  50000  59999   50000-59999
    ## 185 Woman 100000 124999 100000-124999
    ## 186 Woman      0    999         0-999
    ## 187   Man   2000   2999     2000-2999
    ## 188   Man 250000 299999 250000-299999
    ## 189   Man      0    999         0-999
    ## 190   Man  40000  49999   40000-49999
    ## 191   Man   1000   1999     1000-1999
    ## 192   Man  50000  59999   50000-59999
    ## 193   Man   3000   3999     3000-3999
    ## 194   Man      0    999         0-999
    ## 195   Man      0    999         0-999
    ## 196   Man   4000   4999     4000-4999
    ## 197   Man      0    999         0-999
    ## 198   Man  90000  99999   90000-99999
    ## 199   Man  40000  49999   40000-49999
    ## 200   Man      0    999         0-999
    ## 201 Woman 100000 124999 100000-124999
    ## 202   Man  40000  49999   40000-49999
    ## 203   Man  40000  49999   40000-49999
    ## 204 Woman  70000  79999   70000-79999
    ## 205   Man  70000  79999   70000-79999
    ## 206   Man  70000  79999   70000-79999
    ## 207   Man  40000  49999   40000-49999
    ## 208   Man 100000 124999 100000-124999
    ## 209   Man  10000  14999   10000-14999
    ## 210 Woman   3000   3999     3000-3999
    ## 211   Man   1000   1999     1000-1999
    ## 212   Man   3000   3999     3000-3999
    ## 213   Man  30000  39999   30000-39999
    ## 214   Man  15000  19999   15000-19999
    ## 215   Man  80000  89999   80000-89999
    ## 216 Woman  50000  59999   50000-59999
    ## 217   Man 200000 249999 200000-249999
    ## 218   Man   2000   2999     2000-2999
    ## 219 Woman      0    999         0-999
    ## 220   Man   1000   1999     1000-1999
    ## 221 Woman  80000  89999   80000-89999
    ## 222 Woman   7500   9999     7500-9999
    ## 223   Man   7500   9999     7500-9999
    ## 224 Woman 150000 199999 150000-199999
    ## 225   Man   3000   3999     3000-3999
    ## 226   Man  50000  59999   50000-59999
    ## 227   Man   3000   3999     3000-3999
    ## 228   Man  25000  29999   25000-29999
    ## 229   Man  10000  14999   10000-14999
    ## 230   Man  25000  29999   25000-29999
    ## 231   Man  70000  79999   70000-79999
    ## 232   Man   3000   3999     3000-3999
    ## 233   Man   5000   7499     5000-7499
    ## 234 Woman   1000   1999     1000-1999
    ## 235   Man  70000  79999   70000-79999
    ## 236   Man  30000  39999   30000-39999
    ## 237   Man 125000 149999 125000-149999
    ## 238   Man  90000  99999   90000-99999
    ## 239   Man   5000   7499     5000-7499
    ## 240   Man  10000  14999   10000-14999
    ## 241   Man  50000  59999   50000-59999
    ## 242 Woman   1000   1999     1000-1999
    ## 243   Man 150000 199999 150000-199999
    ## 244   Man      0    999         0-999
    ## 245   Man      0    999         0-999
    ## 246   Man 200000 249999 200000-249999
    ## 247   Man      0    999         0-999
    ## 248   Man   7500   9999     7500-9999
    ## 249   Man   2000   2999     2000-2999
    ## 250   Man  80000  89999   80000-89999
    ##  [ reached 'max' / getOption("max.print") -- omitted 7745 rows ]

    gender_paygap <- gender_paygap %>% 
      mutate(
        salary_range = paste0 (
      format(MINsal, trim = TRUE),
      "-", 
      MAXsal
    ),
    salary_range = fct_reorder(salary_range, MINsal)
      )

    #drop the NAs, then rename gender column
    gender_paygap <- gender_paygap %>% drop_na()
    gender_paygap <- gender_paygap %>% rename("gender" =  "Q3")

    gender_paygap %>% arrange(desc(salary_range))

    ##     gender MINsal MAXsal  salary_range
    ## 1      Man 500000 999999 500000-999999
    ## 2      Man 500000 999999 500000-999999
    ## 3      Man 500000 999999 500000-999999
    ## 4      Man 500000 999999 500000-999999
    ## 5      Man 500000 999999 500000-999999
    ## 6      Man 500000 999999 500000-999999
    ## 7      Man 500000 999999 500000-999999
    ## 8    Woman 500000 999999 500000-999999
    ## 9    Woman 500000 999999 500000-999999
    ## 10     Man 500000 999999 500000-999999
    ## 11   Woman 500000 999999 500000-999999
    ## 12     Man 500000 999999 500000-999999
    ## 13     Man 500000 999999 500000-999999
    ## 14   Woman 500000 999999 500000-999999
    ## 15     Man 500000 999999 500000-999999
    ## 16     Man 500000 999999 500000-999999
    ## 17     Man 500000 999999 500000-999999
    ## 18     Man 500000 999999 500000-999999
    ## 19   Woman 500000 999999 500000-999999
    ## 20     Man 500000 999999 500000-999999
    ## 21     Man 500000 999999 500000-999999
    ## 22     Man 500000 999999 500000-999999
    ## 23     Man 500000 999999 500000-999999
    ## 24     Man 500000 999999 500000-999999
    ## 25     Man 500000 999999 500000-999999
    ## 26     Man 500000 999999 500000-999999
    ## 27     Man 500000 999999 500000-999999
    ## 28     Man 500000 999999 500000-999999
    ## 29     Man 500000 999999 500000-999999
    ## 30     Man 500000 999999 500000-999999
    ## 31     Man 500000 999999 500000-999999
    ## 32     Man 500000 999999 500000-999999
    ## 33     Man 500000 999999 500000-999999
    ## 34     Man 500000 999999 500000-999999
    ## 35     Man 500000 999999 500000-999999
    ## 36     Man 500000 999999 500000-999999
    ## 37     Man 500000 999999 500000-999999
    ## 38     Man 500000 999999 500000-999999
    ## 39     Man 500000 999999 500000-999999
    ## 40     Man 500000 999999 500000-999999
    ## 41     Man 500000 999999 500000-999999
    ## 42     Man 500000 999999 500000-999999
    ## 43     Man 500000 999999 500000-999999
    ## 44     Man 500000 999999 500000-999999
    ## 45     Man 500000 999999 500000-999999
    ## 46     Man 500000 999999 500000-999999
    ## 47     Man 500000 999999 500000-999999
    ## 48     Man 500000 999999 500000-999999
    ## 49     Man 300000 499999 300000-499999
    ## 50     Man 300000 499999 300000-499999
    ## 51     Man 300000 499999 300000-499999
    ## 52     Man 300000 499999 300000-499999
    ## 53     Man 300000 499999 300000-499999
    ## 54     Man 300000 499999 300000-499999
    ## 55     Man 300000 499999 300000-499999
    ## 56     Man 300000 499999 300000-499999
    ## 57     Man 300000 499999 300000-499999
    ## 58     Man 300000 499999 300000-499999
    ## 59     Man 300000 499999 300000-499999
    ## 60     Man 300000 499999 300000-499999
    ## 61     Man 300000 499999 300000-499999
    ## 62     Man 300000 499999 300000-499999
    ## 63     Man 300000 499999 300000-499999
    ## 64     Man 300000 499999 300000-499999
    ## 65     Man 300000 499999 300000-499999
    ## 66     Man 300000 499999 300000-499999
    ## 67     Man 300000 499999 300000-499999
    ## 68     Man 300000 499999 300000-499999
    ## 69   Woman 300000 499999 300000-499999
    ## 70     Man 300000 499999 300000-499999
    ## 71     Man 300000 499999 300000-499999
    ## 72     Man 300000 499999 300000-499999
    ## 73     Man 300000 499999 300000-499999
    ## 74     Man 300000 499999 300000-499999
    ## 75     Man 300000 499999 300000-499999
    ## 76     Man 300000 499999 300000-499999
    ## 77     Man 300000 499999 300000-499999
    ## 78   Woman 300000 499999 300000-499999
    ## 79     Man 300000 499999 300000-499999
    ## 80     Man 300000 499999 300000-499999
    ## 81     Man 300000 499999 300000-499999
    ## 82     Man 300000 499999 300000-499999
    ## 83     Man 300000 499999 300000-499999
    ## 84   Woman 300000 499999 300000-499999
    ## 85     Man 300000 499999 300000-499999
    ## 86     Man 300000 499999 300000-499999
    ## 87     Man 300000 499999 300000-499999
    ## 88   Woman 300000 499999 300000-499999
    ## 89     Man 300000 499999 300000-499999
    ## 90     Man 300000 499999 300000-499999
    ## 91     Man 300000 499999 300000-499999
    ## 92     Man 300000 499999 300000-499999
    ## 93     Man 300000 499999 300000-499999
    ## 94     Man 300000 499999 300000-499999
    ## 95     Man 300000 499999 300000-499999
    ## 96     Man 300000 499999 300000-499999
    ## 97     Man 300000 499999 300000-499999
    ## 98     Man 300000 499999 300000-499999
    ## 99     Man 300000 499999 300000-499999
    ## 100    Man 300000 499999 300000-499999
    ## 101    Man 300000 499999 300000-499999
    ## 102  Woman 300000 499999 300000-499999
    ## 103    Man 300000 499999 300000-499999
    ## 104    Man 300000 499999 300000-499999
    ## 105    Man 300000 499999 300000-499999
    ## 106    Man 300000 499999 300000-499999
    ## 107  Woman 300000 499999 300000-499999
    ## 108  Woman 300000 499999 300000-499999
    ## 109    Man 300000 499999 300000-499999
    ## 110  Woman 300000 499999 300000-499999
    ## 111    Man 300000 499999 300000-499999
    ## 112    Man 300000 499999 300000-499999
    ## 113  Woman 300000 499999 300000-499999
    ## 114    Man 300000 499999 300000-499999
    ## 115    Man 300000 499999 300000-499999
    ## 116    Man 300000 499999 300000-499999
    ## 117    Man 300000 499999 300000-499999
    ## 118    Man 300000 499999 300000-499999
    ## 119    Man 300000 499999 300000-499999
    ## 120    Man 300000 499999 300000-499999
    ## 121    Man 250000 299999 250000-299999
    ## 122    Man 250000 299999 250000-299999
    ## 123    Man 250000 299999 250000-299999
    ## 124    Man 250000 299999 250000-299999
    ## 125    Man 250000 299999 250000-299999
    ## 126    Man 250000 299999 250000-299999
    ## 127    Man 250000 299999 250000-299999
    ## 128    Man 250000 299999 250000-299999
    ## 129    Man 250000 299999 250000-299999
    ## 130    Man 250000 299999 250000-299999
    ## 131    Man 250000 299999 250000-299999
    ## 132  Woman 250000 299999 250000-299999
    ## 133    Man 250000 299999 250000-299999
    ## 134  Woman 250000 299999 250000-299999
    ## 135  Woman 250000 299999 250000-299999
    ## 136    Man 250000 299999 250000-299999
    ## 137    Man 250000 299999 250000-299999
    ## 138    Man 250000 299999 250000-299999
    ## 139    Man 250000 299999 250000-299999
    ## 140    Man 250000 299999 250000-299999
    ## 141    Man 250000 299999 250000-299999
    ## 142  Woman 250000 299999 250000-299999
    ## 143    Man 250000 299999 250000-299999
    ## 144  Woman 250000 299999 250000-299999
    ## 145    Man 250000 299999 250000-299999
    ## 146    Man 250000 299999 250000-299999
    ## 147    Man 250000 299999 250000-299999
    ## 148    Man 250000 299999 250000-299999
    ## 149    Man 250000 299999 250000-299999
    ## 150    Man 250000 299999 250000-299999
    ## 151    Man 250000 299999 250000-299999
    ## 152    Man 250000 299999 250000-299999
    ## 153    Man 250000 299999 250000-299999
    ## 154    Man 250000 299999 250000-299999
    ## 155    Man 250000 299999 250000-299999
    ## 156    Man 250000 299999 250000-299999
    ## 157    Man 250000 299999 250000-299999
    ## 158  Woman 250000 299999 250000-299999
    ## 159    Man 250000 299999 250000-299999
    ## 160    Man 250000 299999 250000-299999
    ## 161    Man 250000 299999 250000-299999
    ## 162    Man 250000 299999 250000-299999
    ## 163    Man 250000 299999 250000-299999
    ## 164    Man 250000 299999 250000-299999
    ## 165    Man 250000 299999 250000-299999
    ## 166    Man 250000 299999 250000-299999
    ## 167    Man 250000 299999 250000-299999
    ## 168    Man 250000 299999 250000-299999
    ## 169    Man 250000 299999 250000-299999
    ## 170    Man 250000 299999 250000-299999
    ## 171    Man 250000 299999 250000-299999
    ## 172    Man 250000 299999 250000-299999
    ## 173    Man 250000 299999 250000-299999
    ## 174    Man 250000 299999 250000-299999
    ## 175    Man 250000 299999 250000-299999
    ## 176    Man 250000 299999 250000-299999
    ## 177    Man 250000 299999 250000-299999
    ## 178    Man 250000 299999 250000-299999
    ## 179    Man 250000 299999 250000-299999
    ## 180  Woman 250000 299999 250000-299999
    ## 181    Man 250000 299999 250000-299999
    ## 182    Man 250000 299999 250000-299999
    ## 183    Man 250000 299999 250000-299999
    ## 184    Man 250000 299999 250000-299999
    ## 185    Man 250000 299999 250000-299999
    ## 186  Woman 250000 299999 250000-299999
    ## 187    Man 250000 299999 250000-299999
    ## 188  Woman 250000 299999 250000-299999
    ## 189  Woman 250000 299999 250000-299999
    ## 190    Man 250000 299999 250000-299999
    ## 191    Man 250000 299999 250000-299999
    ## 192    Man 250000 299999 250000-299999
    ## 193    Man 250000 299999 250000-299999
    ## 194    Man 250000 299999 250000-299999
    ## 195    Man 250000 299999 250000-299999
    ## 196    Man 250000 299999 250000-299999
    ## 197    Man 250000 299999 250000-299999
    ## 198    Man 200000 249999 200000-249999
    ## 199    Man 200000 249999 200000-249999
    ## 200    Man 200000 249999 200000-249999
    ## 201    Man 200000 249999 200000-249999
    ## 202    Man 200000 249999 200000-249999
    ## 203    Man 200000 249999 200000-249999
    ## 204    Man 200000 249999 200000-249999
    ## 205  Woman 200000 249999 200000-249999
    ## 206    Man 200000 249999 200000-249999
    ## 207    Man 200000 249999 200000-249999
    ## 208  Woman 200000 249999 200000-249999
    ## 209    Man 200000 249999 200000-249999
    ## 210    Man 200000 249999 200000-249999
    ## 211    Man 200000 249999 200000-249999
    ## 212    Man 200000 249999 200000-249999
    ## 213    Man 200000 249999 200000-249999
    ## 214    Man 200000 249999 200000-249999
    ## 215    Man 200000 249999 200000-249999
    ## 216    Man 200000 249999 200000-249999
    ## 217    Man 200000 249999 200000-249999
    ## 218    Man 200000 249999 200000-249999
    ## 219    Man 200000 249999 200000-249999
    ## 220    Man 200000 249999 200000-249999
    ## 221    Man 200000 249999 200000-249999
    ## 222    Man 200000 249999 200000-249999
    ## 223    Man 200000 249999 200000-249999
    ## 224    Man 200000 249999 200000-249999
    ## 225    Man 200000 249999 200000-249999
    ## 226    Man 200000 249999 200000-249999
    ## 227    Man 200000 249999 200000-249999
    ## 228    Man 200000 249999 200000-249999
    ## 229    Man 200000 249999 200000-249999
    ## 230    Man 200000 249999 200000-249999
    ## 231    Man 200000 249999 200000-249999
    ## 232    Man 200000 249999 200000-249999
    ## 233    Man 200000 249999 200000-249999
    ## 234    Man 200000 249999 200000-249999
    ## 235    Man 200000 249999 200000-249999
    ## 236    Man 200000 249999 200000-249999
    ## 237    Man 200000 249999 200000-249999
    ## 238    Man 200000 249999 200000-249999
    ## 239    Man 200000 249999 200000-249999
    ## 240    Man 200000 249999 200000-249999
    ## 241    Man 200000 249999 200000-249999
    ## 242    Man 200000 249999 200000-249999
    ## 243    Man 200000 249999 200000-249999
    ## 244    Man 200000 249999 200000-249999
    ## 245    Man 200000 249999 200000-249999
    ## 246    Man 200000 249999 200000-249999
    ## 247    Man 200000 249999 200000-249999
    ## 248    Man 200000 249999 200000-249999
    ## 249  Woman 200000 249999 200000-249999
    ## 250    Man 200000 249999 200000-249999
    ##  [ reached 'max' / getOption("max.print") -- omitted 7745 rows ]

    aggr_gpg <- gender_paygap %>% group_by(gender) %>% count(salary_range)

### Mock-up visualizations

    ggplot(aggr_gpg, aes(x=salary_range, y=n, fill=gender)) +
      
    geom_bar(data=subset(aggr_gpg, gender == "Man"), stat="identity") +

    geom_bar(data=subset(aggr_gpg, gender == "Woman"), stat="identity", aes(y=-n)) +
      
    coord_flip() + scale_fill_manual(values=c("lightblue", "pink"))

![](finalproj_files/figure-markdown_strict/unnamed-chunk-34-1.png)

     gender_paygap %>% group_by(gender) %>% summarise(count = n()/nrow(.))

    ## # A tibble: 2 × 2
    ##   gender count
    ##   <chr>  <dbl>
    ## 1 Man    0.827
    ## 2 Woman  0.173

    #gender ratio of gender_paygap df

    gender_demo <- gender_paygap %>% group_by(gender) %>% summarise(percentage = round(n()/nrow(.),4)*100, lab.pos = cumsum(percentage)-.5*percentage)

    ggplot(gender_demo, aes(x=1, y=percentage, fill=gender)) + 
      geom_bar(stat="identity") +
      coord_polar("y", start = 0) +
      geom_text(aes(y = lab.pos, label = paste(percentage,"%", sep = "")), col = "white") + theme_void() +
      scale_fill_manual(values=c("lightblue", "lightpink")) +
      xlim(-1, 2.5)

![](finalproj_files/figure-markdown_strict/unnamed-chunk-37-1.png)

    #ggsave("gender_demo_white.png")

    aggr_gpg2 <- gender_paygap %>% group_by(salary_range, gender) %>% summarise(n = n()) %>% mutate(freq = n /sum(n))

    ## `summarise()` has grouped output by 'salary_range'. You can override using the `.groups` argument.

    ggplot(aggr_gpg2, aes(x=salary_range, y=freq, fill=gender)) +
      
    geom_bar(data=subset(aggr_gpg2, gender == "Man"), stat="identity") +

    geom_bar(data=subset(aggr_gpg2, gender == "Woman"), stat="identity", aes(y=-freq)) +

    geom_hline(yintercept = 0, linetype="dotted", alpha=0.6) +

    # Accuracy of y-axis 
    scale_y_continuous( labels=c("30%","0%","30%","60%","90%"))  +
      
    coord_flip() + scale_fill_manual(values=c("lightblue", "pink")) + theme_minimal() +
      
    labs(x = "salary range (USD)", y = "percent share")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-39-1.png)

    #ggsave("gendersalaryratio.png")     

    gender_paygap %>% group_by(salary_range, gender) %>% summarise(n = n()) %>% mutate(freq = n /sum(n))

    ## `summarise()` has grouped output by 'salary_range'. You can override using the `.groups` argument.

    ## # A tibble: 50 × 4
    ## # Groups:   salary_range [25]
    ##    salary_range gender     n  freq
    ##    <fct>        <chr>  <int> <dbl>
    ##  1 0-999        Man      797 0.729
    ##  2 0-999        Woman    296 0.271
    ##  3 1000-1999    Man      354 0.806
    ##  4 1000-1999    Woman     85 0.194
    ##  5 2000-2999    Man      216 0.806
    ##  6 2000-2999    Woman     52 0.194
    ##  7 3000-3999    Man      185 0.764
    ##  8 3000-3999    Woman     57 0.236
    ##  9 4000-4999    Man      183 0.792
    ## 10 4000-4999    Woman     48 0.208
    ## # … with 40 more rows

    gender_paygap %>% group_by(gender) %>% summarise(n = n()) %>% mutate(freq = n /sum(n))

    ## # A tibble: 2 × 3
    ##   gender     n  freq
    ##   <chr>  <int> <dbl>
    ## 1 Man     6615 0.827
    ## 2 Woman   1380 0.173

    #gender ratio and relative group percentage

## Section A. Gender Dynamics

### Part 2. Job role popularity

#### Data prepping and manipulation

    df1 %>% select(Q23) %>% count(Q23)

    ##                                                                 Q23     n
    ## 1                                                                   13367
    ## 2                                            Currently not employed  1432
    ## 3                                                Data Administrator    70
    ## 4  Data Analyst (Business, Marketing, Financial, Quantitative, etc)  1538
    ## 5                                                    Data Architect    95
    ## 6                                                     Data Engineer   352
    ## 7                                                    Data Scientist  1929
    ## 8                                                Developer Advocate    61
    ## 9                                           Engineer (non-software)   465
    ## 10                                 Machine Learning/ MLops Engineer   571
    ## 11     Manager (Program, Project, Operations, Executive-level, etc)   832
    ## 12                                                            Other   754
    ## 13                                               Research Scientist   593
    ## 14                                                Software Engineer   980
    ## 15                                                     Statistician   125
    ## 16                                              Teacher / professor   833

    gender_job <- df1 %>% select(c("Q3", "Q23"))
    gender_job <- gender_job %>% mutate_all(na_if, "")
    gender_job <- gender_job %>% filter(Q3 == "Man" | Q3 == "Woman")
    gender_job <- gender_job %>% filter(Q23 != "Currently not employed" & Q23 != "Other")
    gender_job <- gender_job %>% drop_na()
    #house cleaning code chunk

    gender_job <- gender_job %>% mutate(Q23 = replace(Q23, Q23 == "Data Analyst (Business, Marketing, Financial, Quantitative, etc)", "Data Analyst")) %>% mutate(Q23 = replace(Q23, Q23 == "Manager (Program, Project, Operations, Executive-level, etc)", "Manager")) 
    #simplifying strings of some job roles

    gender_job <- gender_job %>% rename("gender" = "Q3", "job_title" = "Q23")

    gender_job %>% select(job_title) %>% count(job_title) %>% arrange(desc(n))

    ##                           job_title    n
    ## 1                    Data Scientist 1896
    ## 2                      Data Analyst 1522
    ## 3                 Software Engineer  966
    ## 4               Teacher / professor  823
    ## 5                           Manager  816
    ## 6                Research Scientist  583
    ## 7  Machine Learning/ MLops Engineer  558
    ## 8           Engineer (non-software)  460
    ## 9                     Data Engineer  342
    ## 10                     Statistician  125
    ## 11                   Data Architect   90
    ## 12               Data Administrator   70
    ## 13               Developer Advocate   59

##### Group statistics

    gender_job %>% group_by(job_title) %>% summarise(n = n()) %>% mutate(freq = n /sum(n)) %>% arrange(desc(n))

    ## # A tibble: 13 × 3
    ##    job_title                            n    freq
    ##    <chr>                            <int>   <dbl>
    ##  1 Data Scientist                    1896 0.228  
    ##  2 Data Analyst                      1522 0.183  
    ##  3 Software Engineer                  966 0.116  
    ##  4 Teacher / professor                823 0.0990 
    ##  5 Manager                            816 0.0982 
    ##  6 Research Scientist                 583 0.0702 
    ##  7 Machine Learning/ MLops Engineer   558 0.0671 
    ##  8 Engineer (non-software)            460 0.0554 
    ##  9 Data Engineer                      342 0.0412 
    ## 10 Statistician                       125 0.0150 
    ## 11 Data Architect                      90 0.0108 
    ## 12 Data Administrator                  70 0.00842
    ## 13 Developer Advocate                  59 0.00710

    gender_job_ratio <- gender_job %>% group_by(job_title, gender) %>% summarise(n = n()) %>% mutate(freq = n /sum(n))

    ## `summarise()` has grouped output by 'job_title'. You can override using the `.groups` argument.

    gender_job %>% group_by(job_title, gender) %>% summarise(n = n()) %>% mutate(freq = n /sum(n))

    ## `summarise()` has grouped output by 'job_title'. You can override using the `.groups` argument.

    ## # A tibble: 26 × 4
    ## # Groups:   job_title [13]
    ##    job_title          gender     n   freq
    ##    <chr>              <chr>  <int>  <dbl>
    ##  1 Data Administrator Man       56 0.8   
    ##  2 Data Administrator Woman     14 0.2   
    ##  3 Data Analyst       Man     1188 0.781 
    ##  4 Data Analyst       Woman    334 0.219 
    ##  5 Data Architect     Man       82 0.911 
    ##  6 Data Architect     Woman      8 0.0889
    ##  7 Data Engineer      Man      292 0.854 
    ##  8 Data Engineer      Woman     50 0.146 
    ##  9 Data Scientist     Man     1588 0.838 
    ## 10 Data Scientist     Woman    308 0.162 
    ## # … with 16 more rows

### Mock-up visualization of gender-job ratio

    ggplot(gender_job_ratio, aes(x=job_title, y=freq, fill=gender)) +
      
    geom_bar(data=subset(gender_job_ratio, gender == "Man"), stat="identity") +

    geom_bar(data=subset(gender_job_ratio, gender == "Woman"), stat="identity", aes(y=-freq)) +

    geom_hline(yintercept = 0, linetype="dotted", alpha=0.6) +
    # Accuracy of y-axis
    scale_y_continuous(breaks=c(-0.25,0,0.25,0.5,0.75),labels=c("25%", "0%", "25%","50%", "75%")) +

    coord_flip() + scale_fill_manual(values=c("lightblue", "pink")) + theme_minimal() +
      
    labs(x="job title", y="percent share")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-46-1.png)

    #ggsave("genderjobratio.png")

#### cloud computing exploration

    cc_usage <- df1 %>% select(contains(c("Q31")))

    cc_enjoyability <- df1 %>% select(Q32)

    cc_enjoyability %>% count(Q32) %>% arrange(desc(n))

    ##                                                        Q32     n
    ## 1                                                          22068
    ## 2                               Amazon Web Services (AWS)    555
    ## 3                             Google Cloud Platform (GCP)    501
    ## 4  They all had a similarly enjoyable developer experience   443
    ## 5                                         Microsoft Azure    256
    ## 6                                   None were satisfactory    72
    ## 7                                     IBM Cloud / Red Hat     34
    ## 8                                            Oracle Cloud     20
    ## 9                                                    Other    20
    ## 10                                           VMware Cloud     12
    ## 11                                              SAP Cloud      7
    ## 12                                          Alibaba Cloud      5
    ## 13                                          Tencent Cloud      3
    ## 14                                           Huawei Cloud      1

    cc_usage %>% gather("key", "value") %>% group_by(value) %>% summarise(n=n()) %>% arrange(desc(n))

    ## # A tibble: 13 × 2
    ##    value                                n
    ##    <chr>                            <int>
    ##  1 ""                              279804
    ##  2 " Amazon Web Services (AWS) "     2346
    ##  3 " Google Cloud Platform (GCP) "   2056
    ##  4 " Microsoft Azure "               1416
    ##  5 "None"                            1167
    ##  6 " IBM Cloud / Red Hat "            287
    ##  7 " Oracle Cloud "                   230
    ##  8 "Other"                            217
    ##  9 " VMware Cloud "                   155
    ## 10 " SAP Cloud "                      107
    ## 11 " Alibaba Cloud "                   76
    ## 12 " Tencent Cloud "                   56
    ## 13 " Huawei Cloud "                    47

## cc spending

    cc_spending <- df1 %>% select(c(Q30, Q23))

    df1 %>% select(contains(c("Q44"))) %>% gather("key", "value") %>% group_by(value) %>% summarise(n=n()) %>% arrange(desc(n))

    ## # A tibble: 13 × 2
    ##    value                                                                             n
    ##    <chr>                                                                         <int>
    ##  1 ""                                                                           232841
    ##  2 "YouTube (Kaggle YouTube, Cloud AI Adventures, etc)"                          11957
    ##  3 "Kaggle (notebooks, forums, etc)"                                             11181
    ##  4 "Blogs (Towards Data Science, Analytics Vidhya, etc)"                          7766
    ##  5 "Course Forums (forums.fast.ai, Coursera forums, etc)"                         4006
    ##  6 "Twitter (data science influencers)"                                           3995
    ##  7 "Journal Publications (peer-reviewed journals, conference proceedings, etc)"   3804
    ##  8 "Email newsletters (Data Elixir, O'Reilly Data & AI, etc)"                     3787
    ##  9 "Reddit (r/machinelearning, etc)"                                              2678
    ## 10 "Podcasts (Chai Time Data Science, O’Reilly Data Show, etc)"                   2120
    ## 11 "Slack Communities (ods.ai, kagglenoobs, etc)"                                 1726
    ## 12 "None"                                                                         1268
    ## 13 "Other"                                                                         835

\### distribution of salary range

    salary_range_dist <- gender_paygap %>% select(salary_range) %>% count(salary_range)

    ggplot(salary_range_dist, aes(x=salary_range, y=n)) +

      geom_col(fill="skyblue") +
      
      coord_flip() + theme_minimal()

![](finalproj_files/figure-markdown_strict/unnamed-chunk-54-1.png)

plot above is skewed, need to consolidate to USA-only

next, tidying up data

    clean_sal <- df1 %>% select(c("Q4","Q29")) %>% mutate(across(starts_with("Q29"), ~gsub("\\$", "", .))) %>% mutate(across(starts_with("Q29"), ~gsub("\\,", "", .)))
    #strip out $ and commas

    clean_sal <- clean_sal  %>% filter(Q29 != ">1000000")
    #remove millionaires

    clean_sal<- clean_sal %>% separate(Q29, c("MINsal", "MAXsal"))

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 15861 rows [1, 2, 3, 5, 6, 7, 10, 11, 12, 13, 15, 16,
    ## 22, 23, 24, 25, 27, 29, 31, 32, ...].

    #string split by "-"

    clean_sal

    ##                                                       Q4 MINsal MAXsal
    ## 1                                                  India          <NA>
    ## 2                                                Algeria          <NA>
    ## 3                                                  Egypt          <NA>
    ## 4                                                 France  25000  29999
    ## 5                                                  India          <NA>
    ## 6                                                  India          <NA>
    ## 7                                                  India          <NA>
    ## 8                                                Germany 100000 124999
    ## 9                                              Australia 100000 124999
    ## 10                                                 Other          <NA>
    ## 11                                           South Korea          <NA>
    ## 12                                                 Egypt          <NA>
    ## 13                                                 India          <NA>
    ## 14                              United States of America 200000 249999
    ## 15                                              Pakistan          <NA>
    ## 16                                                Mexico          <NA>
    ## 17                              United States of America 200000 249999
    ## 18                              United States of America 150000 199999
    ## 19                                             Singapore  90000  99999
    ## 20                                                 Italy  30000  39999
    ## 21                                                Taiwan  30000  39999
    ## 22                                                 China          <NA>
    ## 23                                                 India          <NA>
    ## 24                                             Argentina          <NA>
    ## 25                                                Turkey          <NA>
    ## 26                                                Brazil  30000  39999
    ## 27                                               Nigeria          <NA>
    ## 28                                             Argentina   3000   3999
    ## 29                              United States of America          <NA>
    ## 30                                                 Chile  50000  59999
    ## 31                                             Singapore          <NA>
    ## 32                                                 Other          <NA>
    ## 33                                                 Other 125000 149999
    ## 34                                                 Italy  15000  19999
    ## 35                                                 India          <NA>
    ## 36                                                 Japan  50000  59999
    ## 37                                                 India          <NA>
    ## 38                                                Mexico          <NA>
    ## 39                                                 India          <NA>
    ## 40                                                 India          <NA>
    ## 41                                                Canada   5000   7499
    ## 42                                             Indonesia  10000  14999
    ## 43                                                 India          <NA>
    ## 44                                                 India  30000  39999
    ## 45                                                Israel          <NA>
    ## 46                                              Colombia  20000  24999
    ## 47                                                 India          <NA>
    ## 48                                                 India          <NA>
    ## 49                                                France   3000   3999
    ## 50                                                Brazil          <NA>
    ## 51                                                 Other          <NA>
    ## 52                                                 India          <NA>
    ## 53                                             Indonesia          <NA>
    ## 54                                                 Egypt          <NA>
    ## 55                                                Turkey          <NA>
    ## 56                                                 India          <NA>
    ## 57                                                 Egypt          <NA>
    ## 58                                                 India  25000  29999
    ## 59                                                Turkey          <NA>
    ## 60                                              Pakistan          <NA>
    ## 61                                                 India          <NA>
    ## 62                                              Pakistan          <NA>
    ## 63                                                Russia          <NA>
    ## 64                                              Pakistan      0    999
    ## 65                                                 India          <NA>
    ## 66                                                 Other          <NA>
    ## 67                                              Colombia          <NA>
    ## 68                                                 India          <NA>
    ## 69                                                 India          <NA>
    ## 70                                               Nigeria          <NA>
    ## 71                                                 China          <NA>
    ## 72                                                 Japan  50000  59999
    ## 73  United Kingdom of Great Britain and Northern Ireland          <NA>
    ## 74                                                Canada          <NA>
    ## 75                                                 India   3000   3999
    ## 76                                                 India   7500   9999
    ## 77                                                Russia          <NA>
    ## 78                                                 India          <NA>
    ## 79                                                 India          <NA>
    ## 80                                                 Other   4000   4999
    ## 81                                                 India          <NA>
    ## 82                                                Canada 100000 124999
    ## 83                              United States of America  25000  29999
    ## 84                                                 Japan      0    999
    ## 85                              United States of America  80000  89999
    ## 86                                                 India          <NA>
    ## 87                                              Ethiopia          <NA>
    ## 88                                                 India          <NA>
    ## 89                                                 India          <NA>
    ## 90                                                 India          <NA>
    ## 91                                                 Egypt          <NA>
    ## 92                                          South Africa          <NA>
    ## 93                                                 India          <NA>
    ## 94                                                France          <NA>
    ## 95                                                 India          <NA>
    ## 96                              United States of America          <NA>
    ## 97                                                 India          <NA>
    ## 98                                                 India  20000  24999
    ## 99                                                 India  15000  19999
    ## 100                                                India          <NA>
    ## 101                                               Turkey          <NA>
    ## 102                                                India          <NA>
    ## 103                                             Pakistan   3000   3999
    ## 104                                            Indonesia          <NA>
    ## 105                             United States of America          <NA>
    ## 106                                             Pakistan          <NA>
    ## 107                                          South Korea          <NA>
    ## 108                                             Pakistan          <NA>
    ## 109                                             Viet Nam          <NA>
    ## 110                                                India          <NA>
    ## 111                                               Israel          <NA>
    ## 112                                             Viet Nam          <NA>
    ## 113                                                India          <NA>
    ## 114                                                India          <NA>
    ## 115                                                India      0    999
    ## 116                                                India          <NA>
    ## 117                                                India          <NA>
    ## 118                                                Chile          <NA>
    ## 119                                                India          <NA>
    ## 120                                               Canada  90000  99999
    ## 121                                                China          <NA>
    ## 122                                               Mexico      0    999
    ## 123                                          South Korea          <NA>
    ## 124                                                Japan   4000   4999
    ## 125                                                India          <NA>
    ## 126                                                Other   3000   3999
    ## 127                             United States of America          <NA>
    ## 128                                            Australia 125000 149999
    ## 129                                                India          <NA>
    ## 130                             United States of America          <NA>
    ## 131                                                Italy          <NA>
    ## 132                                                Italy          <NA>
    ## 133                                            Indonesia          <NA>
    ## 134                                             Colombia  50000  59999
    ## 135                                               Brazil          <NA>
    ## 136                                               Turkey  30000  39999
    ## 137                                                India  30000  39999
    ## 138                                                Kenya          <NA>
    ## 139                                                Japan          <NA>
    ## 140                                                Spain 150000 199999
    ## 141                                                India          <NA>
    ## 142                         Iran, Islamic Republic of...          <NA>
    ## 143                                                India   3000   3999
    ## 144                                             Cameroon   7500   9999
    ## 145                                               Brazil          <NA>
    ## 146                             United States of America          <NA>
    ## 147                                             Pakistan          <NA>
    ## 148                                               Brazil          <NA>
    ## 149                                             Pakistan          <NA>
    ## 150                                                India          <NA>
    ## 151                                               Brazil          <NA>
    ## 152                                                India          <NA>
    ## 153                                                India          <NA>
    ## 154                                               Israel          <NA>
    ## 155                                                Spain  50000  59999
    ## 156                                                China          <NA>
    ## 157                                                India      0    999
    ## 158                                                India          <NA>
    ## 159                                              Nigeria      0    999
    ## 160                                                Japan  20000  24999
    ## 161                                                India   4000   4999
    ## 162                                                India          <NA>
    ## 163                                               Turkey      0    999
    ## 164                             United States of America 100000 124999
    ## 165                                                India   3000   3999
    ## 166                                                India          <NA>
    ## 167                                                Italy          <NA>
    ## 168                                              Tunisia          <NA>
    ## 169                                                Other          <NA>
    ## 170                             United States of America          <NA>
    ## 171                                              Nigeria          <NA>
    ## 172                                                 Peru          <NA>
    ## 173                                                India   2000   2999
    ## 174                             United States of America          <NA>
    ## 175                                                India          <NA>
    ## 176                                                India          <NA>
    ## 177                                          South Korea          <NA>
    ## 178                                              Germany      0    999
    ## 179                                                India          <NA>
    ## 180                                             Colombia          <NA>
    ## 181                                                India          <NA>
    ## 182                                             Pakistan          <NA>
    ## 183                                               Russia          <NA>
    ## 184                                                Nepal          <NA>
    ## 185                                                Other          <NA>
    ## 186                                                India      0    999
    ## 187                                                Spain          <NA>
    ## 188                             United States of America          <NA>
    ## 189                                              Morocco          <NA>
    ## 190                                                India          <NA>
    ## 191                                             Colombia          <NA>
    ## 192                                             Colombia          <NA>
    ## 193                             United States of America          <NA>
    ## 194                                                Japan  80000  89999
    ## 195                             United States of America          <NA>
    ## 196                                                India          <NA>
    ## 197                                            Indonesia          <NA>
    ## 198                                                India  30000  39999
    ## 199                                              Nigeria          <NA>
    ## 200                                                India          <NA>
    ## 201                             United States of America 250000 299999
    ## 202                                                Other  20000  24999
    ## 203                                                India      0    999
    ## 204                                                India   3000   3999
    ## 205                                            Argentina          <NA>
    ## 206                             United States of America  80000  89999
    ## 207                                                China          <NA>
    ## 208                             United States of America  90000  99999
    ## 209                                                China          <NA>
    ## 210                                                India          <NA>
    ## 211                             United States of America 150000 199999
    ## 212                                                 Peru          <NA>
    ## 213                                               Turkey  30000  39999
    ## 214                                                India   4000   4999
    ## 215                                                India          <NA>
    ## 216                                                India          <NA>
    ## 217                                                India      0    999
    ## 218                                                India          <NA>
    ## 219                                         South Africa          <NA>
    ## 220                                                Japan 125000 149999
    ## 221                                                Other          <NA>
    ## 222                                              Nigeria          <NA>
    ## 223                                                India          <NA>
    ## 224                                               Turkey   1000   1999
    ## 225                                                India          <NA>
    ## 226                                                India          <NA>
    ## 227                                             Colombia          <NA>
    ## 228                                                China          <NA>
    ## 229                                                India          <NA>
    ## 230                                                India   3000   3999
    ## 231                                                India          <NA>
    ## 232                                                India          <NA>
    ## 233                                               Canada          <NA>
    ## 234                                                India          <NA>
    ## 235                                                Nepal          <NA>
    ## 236                                               Taiwan          <NA>
    ## 237                                                Other          <NA>
    ## 238 United Kingdom of Great Britain and Northern Ireland 500000 999999
    ## 239                                                Japan          <NA>
    ## 240                                                India          <NA>
    ## 241                             United States of America 150000 199999
    ## 242                                                India          <NA>
    ## 243                                               Israel          <NA>
    ## 244                                                India   7500   9999
    ## 245                                                India  70000  79999
    ## 246                             United States of America          <NA>
    ## 247                                             Ethiopia   3000   3999
    ## 248                                                Other          <NA>
    ## 249                                               Canada          <NA>
    ## 250                                            Argentina          <NA>
    ## 251                                               Turkey   7500   9999
    ## 252                                              Nigeria          <NA>
    ## 253                                                Kenya          <NA>
    ## 254                                                India          <NA>
    ## 255                                              Tunisia      0    999
    ## 256                                                India          <NA>
    ## 257                                             Colombia          <NA>
    ## 258                                          Philippines  10000  14999
    ## 259                                                India          <NA>
    ## 260                                                India          <NA>
    ## 261                                                Japan          <NA>
    ## 262                                                Egypt  10000  14999
    ## 263                                                India          <NA>
    ## 264                                                Other          <NA>
    ## 265                                                Chile      0    999
    ## 266                                               Brazil          <NA>
    ## 267                                                India          <NA>
    ## 268                                                India   7500   9999
    ## 269                                               Brazil  30000  39999
    ## 270                                             Colombia  10000  14999
    ## 271                                              Ecuador          <NA>
    ## 272                                                Japan  15000  19999
    ## 273                                                India          <NA>
    ## 274                                          South Korea 100000 124999
    ## 275                             United States of America          <NA>
    ## 276                                              Nigeria          <NA>
    ## 277                                                India          <NA>
    ## 278                                             Viet Nam          <NA>
    ## 279                             United States of America      0    999
    ## 280                             United States of America          <NA>
    ## 281                                                India          <NA>
    ## 282                                                India   5000   7499
    ## 283                                            Indonesia  50000  59999
    ## 284                                                India          <NA>
    ## 285                                                India          <NA>
    ## 286                                                Japan          <NA>
    ## 287                                                India          <NA>
    ## 288                                                India          <NA>
    ## 289                                                India          <NA>
    ## 290                                                Japan 100000 124999
    ## 291                                                India  10000  14999
    ## 292                                               Canada          <NA>
    ## 293                                             Pakistan          <NA>
    ## 294                                                Chile          <NA>
    ## 295                                                India          <NA>
    ## 296                                                India          <NA>
    ## 297                                                India   7500   9999
    ## 298                                                India          <NA>
    ## 299                                                 Peru   1000   1999
    ## 300                                                India          <NA>
    ## 301                                                India          <NA>
    ## 302                                                India          <NA>
    ## 303                                                Other          <NA>
    ## 304                                             Thailand  50000  59999
    ## 305                                                India          <NA>
    ## 306                                                Egypt          <NA>
    ## 307                                 United Arab Emirates          <NA>
    ## 308                                                India          <NA>
    ## 309                                                Egypt      0    999
    ## 310                                                India          <NA>
    ## 311                                                Spain  30000  39999
    ## 312                                                India          <NA>
    ## 313                                             Colombia          <NA>
    ## 314                                                India  20000  24999
    ## 315                                               Israel          <NA>
    ## 316                                                India          <NA>
    ## 317                                                India          <NA>
    ## 318                             United States of America          <NA>
    ## 319                             United States of America  60000  69999
    ## 320                                                India          <NA>
    ## 321                                                Japan  40000  49999
    ## 322                                                India   1000   1999
    ## 323                                                India          <NA>
    ## 324                                             Colombia          <NA>
    ## 325                                                India          <NA>
    ## 326                                              Germany          <NA>
    ## 327                                             Cameroon   5000   7499
    ## 328                             United States of America          <NA>
    ## 329                                                Egypt      0    999
    ## 330                                                India          <NA>
    ## 331                         Iran, Islamic Republic of...      0    999
    ## 332                                                India          <NA>
    ## 333                                           Bangladesh          <NA>
    ##  [ reached 'max' / getOption("max.print") -- omitted 23641 rows ]

bind clean salaries

    global_sal_range <- clean_sal

    global_sal_range

    ##                                                       Q4 MINsal MAXsal
    ## 1                                                  India          <NA>
    ## 2                                                Algeria          <NA>
    ## 3                                                  Egypt          <NA>
    ## 4                                                 France  25000  29999
    ## 5                                                  India          <NA>
    ## 6                                                  India          <NA>
    ## 7                                                  India          <NA>
    ## 8                                                Germany 100000 124999
    ## 9                                              Australia 100000 124999
    ## 10                                                 Other          <NA>
    ## 11                                           South Korea          <NA>
    ## 12                                                 Egypt          <NA>
    ## 13                                                 India          <NA>
    ## 14                              United States of America 200000 249999
    ## 15                                              Pakistan          <NA>
    ## 16                                                Mexico          <NA>
    ## 17                              United States of America 200000 249999
    ## 18                              United States of America 150000 199999
    ## 19                                             Singapore  90000  99999
    ## 20                                                 Italy  30000  39999
    ## 21                                                Taiwan  30000  39999
    ## 22                                                 China          <NA>
    ## 23                                                 India          <NA>
    ## 24                                             Argentina          <NA>
    ## 25                                                Turkey          <NA>
    ## 26                                                Brazil  30000  39999
    ## 27                                               Nigeria          <NA>
    ## 28                                             Argentina   3000   3999
    ## 29                              United States of America          <NA>
    ## 30                                                 Chile  50000  59999
    ## 31                                             Singapore          <NA>
    ## 32                                                 Other          <NA>
    ## 33                                                 Other 125000 149999
    ## 34                                                 Italy  15000  19999
    ## 35                                                 India          <NA>
    ## 36                                                 Japan  50000  59999
    ## 37                                                 India          <NA>
    ## 38                                                Mexico          <NA>
    ## 39                                                 India          <NA>
    ## 40                                                 India          <NA>
    ## 41                                                Canada   5000   7499
    ## 42                                             Indonesia  10000  14999
    ## 43                                                 India          <NA>
    ## 44                                                 India  30000  39999
    ## 45                                                Israel          <NA>
    ## 46                                              Colombia  20000  24999
    ## 47                                                 India          <NA>
    ## 48                                                 India          <NA>
    ## 49                                                France   3000   3999
    ## 50                                                Brazil          <NA>
    ## 51                                                 Other          <NA>
    ## 52                                                 India          <NA>
    ## 53                                             Indonesia          <NA>
    ## 54                                                 Egypt          <NA>
    ## 55                                                Turkey          <NA>
    ## 56                                                 India          <NA>
    ## 57                                                 Egypt          <NA>
    ## 58                                                 India  25000  29999
    ## 59                                                Turkey          <NA>
    ## 60                                              Pakistan          <NA>
    ## 61                                                 India          <NA>
    ## 62                                              Pakistan          <NA>
    ## 63                                                Russia          <NA>
    ## 64                                              Pakistan      0    999
    ## 65                                                 India          <NA>
    ## 66                                                 Other          <NA>
    ## 67                                              Colombia          <NA>
    ## 68                                                 India          <NA>
    ## 69                                                 India          <NA>
    ## 70                                               Nigeria          <NA>
    ## 71                                                 China          <NA>
    ## 72                                                 Japan  50000  59999
    ## 73  United Kingdom of Great Britain and Northern Ireland          <NA>
    ## 74                                                Canada          <NA>
    ## 75                                                 India   3000   3999
    ## 76                                                 India   7500   9999
    ## 77                                                Russia          <NA>
    ## 78                                                 India          <NA>
    ## 79                                                 India          <NA>
    ## 80                                                 Other   4000   4999
    ## 81                                                 India          <NA>
    ## 82                                                Canada 100000 124999
    ## 83                              United States of America  25000  29999
    ## 84                                                 Japan      0    999
    ## 85                              United States of America  80000  89999
    ## 86                                                 India          <NA>
    ## 87                                              Ethiopia          <NA>
    ## 88                                                 India          <NA>
    ## 89                                                 India          <NA>
    ## 90                                                 India          <NA>
    ## 91                                                 Egypt          <NA>
    ## 92                                          South Africa          <NA>
    ## 93                                                 India          <NA>
    ## 94                                                France          <NA>
    ## 95                                                 India          <NA>
    ## 96                              United States of America          <NA>
    ## 97                                                 India          <NA>
    ## 98                                                 India  20000  24999
    ## 99                                                 India  15000  19999
    ## 100                                                India          <NA>
    ## 101                                               Turkey          <NA>
    ## 102                                                India          <NA>
    ## 103                                             Pakistan   3000   3999
    ## 104                                            Indonesia          <NA>
    ## 105                             United States of America          <NA>
    ## 106                                             Pakistan          <NA>
    ## 107                                          South Korea          <NA>
    ## 108                                             Pakistan          <NA>
    ## 109                                             Viet Nam          <NA>
    ## 110                                                India          <NA>
    ## 111                                               Israel          <NA>
    ## 112                                             Viet Nam          <NA>
    ## 113                                                India          <NA>
    ## 114                                                India          <NA>
    ## 115                                                India      0    999
    ## 116                                                India          <NA>
    ## 117                                                India          <NA>
    ## 118                                                Chile          <NA>
    ## 119                                                India          <NA>
    ## 120                                               Canada  90000  99999
    ## 121                                                China          <NA>
    ## 122                                               Mexico      0    999
    ## 123                                          South Korea          <NA>
    ## 124                                                Japan   4000   4999
    ## 125                                                India          <NA>
    ## 126                                                Other   3000   3999
    ## 127                             United States of America          <NA>
    ## 128                                            Australia 125000 149999
    ## 129                                                India          <NA>
    ## 130                             United States of America          <NA>
    ## 131                                                Italy          <NA>
    ## 132                                                Italy          <NA>
    ## 133                                            Indonesia          <NA>
    ## 134                                             Colombia  50000  59999
    ## 135                                               Brazil          <NA>
    ## 136                                               Turkey  30000  39999
    ## 137                                                India  30000  39999
    ## 138                                                Kenya          <NA>
    ## 139                                                Japan          <NA>
    ## 140                                                Spain 150000 199999
    ## 141                                                India          <NA>
    ## 142                         Iran, Islamic Republic of...          <NA>
    ## 143                                                India   3000   3999
    ## 144                                             Cameroon   7500   9999
    ## 145                                               Brazil          <NA>
    ## 146                             United States of America          <NA>
    ## 147                                             Pakistan          <NA>
    ## 148                                               Brazil          <NA>
    ## 149                                             Pakistan          <NA>
    ## 150                                                India          <NA>
    ## 151                                               Brazil          <NA>
    ## 152                                                India          <NA>
    ## 153                                                India          <NA>
    ## 154                                               Israel          <NA>
    ## 155                                                Spain  50000  59999
    ## 156                                                China          <NA>
    ## 157                                                India      0    999
    ## 158                                                India          <NA>
    ## 159                                              Nigeria      0    999
    ## 160                                                Japan  20000  24999
    ## 161                                                India   4000   4999
    ## 162                                                India          <NA>
    ## 163                                               Turkey      0    999
    ## 164                             United States of America 100000 124999
    ## 165                                                India   3000   3999
    ## 166                                                India          <NA>
    ## 167                                                Italy          <NA>
    ## 168                                              Tunisia          <NA>
    ## 169                                                Other          <NA>
    ## 170                             United States of America          <NA>
    ## 171                                              Nigeria          <NA>
    ## 172                                                 Peru          <NA>
    ## 173                                                India   2000   2999
    ## 174                             United States of America          <NA>
    ## 175                                                India          <NA>
    ## 176                                                India          <NA>
    ## 177                                          South Korea          <NA>
    ## 178                                              Germany      0    999
    ## 179                                                India          <NA>
    ## 180                                             Colombia          <NA>
    ## 181                                                India          <NA>
    ## 182                                             Pakistan          <NA>
    ## 183                                               Russia          <NA>
    ## 184                                                Nepal          <NA>
    ## 185                                                Other          <NA>
    ## 186                                                India      0    999
    ## 187                                                Spain          <NA>
    ## 188                             United States of America          <NA>
    ## 189                                              Morocco          <NA>
    ## 190                                                India          <NA>
    ## 191                                             Colombia          <NA>
    ## 192                                             Colombia          <NA>
    ## 193                             United States of America          <NA>
    ## 194                                                Japan  80000  89999
    ## 195                             United States of America          <NA>
    ## 196                                                India          <NA>
    ## 197                                            Indonesia          <NA>
    ## 198                                                India  30000  39999
    ## 199                                              Nigeria          <NA>
    ## 200                                                India          <NA>
    ## 201                             United States of America 250000 299999
    ## 202                                                Other  20000  24999
    ## 203                                                India      0    999
    ## 204                                                India   3000   3999
    ## 205                                            Argentina          <NA>
    ## 206                             United States of America  80000  89999
    ## 207                                                China          <NA>
    ## 208                             United States of America  90000  99999
    ## 209                                                China          <NA>
    ## 210                                                India          <NA>
    ## 211                             United States of America 150000 199999
    ## 212                                                 Peru          <NA>
    ## 213                                               Turkey  30000  39999
    ## 214                                                India   4000   4999
    ## 215                                                India          <NA>
    ## 216                                                India          <NA>
    ## 217                                                India      0    999
    ## 218                                                India          <NA>
    ## 219                                         South Africa          <NA>
    ## 220                                                Japan 125000 149999
    ## 221                                                Other          <NA>
    ## 222                                              Nigeria          <NA>
    ## 223                                                India          <NA>
    ## 224                                               Turkey   1000   1999
    ## 225                                                India          <NA>
    ## 226                                                India          <NA>
    ## 227                                             Colombia          <NA>
    ## 228                                                China          <NA>
    ## 229                                                India          <NA>
    ## 230                                                India   3000   3999
    ## 231                                                India          <NA>
    ## 232                                                India          <NA>
    ## 233                                               Canada          <NA>
    ## 234                                                India          <NA>
    ## 235                                                Nepal          <NA>
    ## 236                                               Taiwan          <NA>
    ## 237                                                Other          <NA>
    ## 238 United Kingdom of Great Britain and Northern Ireland 500000 999999
    ## 239                                                Japan          <NA>
    ## 240                                                India          <NA>
    ## 241                             United States of America 150000 199999
    ## 242                                                India          <NA>
    ## 243                                               Israel          <NA>
    ## 244                                                India   7500   9999
    ## 245                                                India  70000  79999
    ## 246                             United States of America          <NA>
    ## 247                                             Ethiopia   3000   3999
    ## 248                                                Other          <NA>
    ## 249                                               Canada          <NA>
    ## 250                                            Argentina          <NA>
    ## 251                                               Turkey   7500   9999
    ## 252                                              Nigeria          <NA>
    ## 253                                                Kenya          <NA>
    ## 254                                                India          <NA>
    ## 255                                              Tunisia      0    999
    ## 256                                                India          <NA>
    ## 257                                             Colombia          <NA>
    ## 258                                          Philippines  10000  14999
    ## 259                                                India          <NA>
    ## 260                                                India          <NA>
    ## 261                                                Japan          <NA>
    ## 262                                                Egypt  10000  14999
    ## 263                                                India          <NA>
    ## 264                                                Other          <NA>
    ## 265                                                Chile      0    999
    ## 266                                               Brazil          <NA>
    ## 267                                                India          <NA>
    ## 268                                                India   7500   9999
    ## 269                                               Brazil  30000  39999
    ## 270                                             Colombia  10000  14999
    ## 271                                              Ecuador          <NA>
    ## 272                                                Japan  15000  19999
    ## 273                                                India          <NA>
    ## 274                                          South Korea 100000 124999
    ## 275                             United States of America          <NA>
    ## 276                                              Nigeria          <NA>
    ## 277                                                India          <NA>
    ## 278                                             Viet Nam          <NA>
    ## 279                             United States of America      0    999
    ## 280                             United States of America          <NA>
    ## 281                                                India          <NA>
    ## 282                                                India   5000   7499
    ## 283                                            Indonesia  50000  59999
    ## 284                                                India          <NA>
    ## 285                                                India          <NA>
    ## 286                                                Japan          <NA>
    ## 287                                                India          <NA>
    ## 288                                                India          <NA>
    ## 289                                                India          <NA>
    ## 290                                                Japan 100000 124999
    ## 291                                                India  10000  14999
    ## 292                                               Canada          <NA>
    ## 293                                             Pakistan          <NA>
    ## 294                                                Chile          <NA>
    ## 295                                                India          <NA>
    ## 296                                                India          <NA>
    ## 297                                                India   7500   9999
    ## 298                                                India          <NA>
    ## 299                                                 Peru   1000   1999
    ## 300                                                India          <NA>
    ## 301                                                India          <NA>
    ## 302                                                India          <NA>
    ## 303                                                Other          <NA>
    ## 304                                             Thailand  50000  59999
    ## 305                                                India          <NA>
    ## 306                                                Egypt          <NA>
    ## 307                                 United Arab Emirates          <NA>
    ## 308                                                India          <NA>
    ## 309                                                Egypt      0    999
    ## 310                                                India          <NA>
    ## 311                                                Spain  30000  39999
    ## 312                                                India          <NA>
    ## 313                                             Colombia          <NA>
    ## 314                                                India  20000  24999
    ## 315                                               Israel          <NA>
    ## 316                                                India          <NA>
    ## 317                                                India          <NA>
    ## 318                             United States of America          <NA>
    ## 319                             United States of America  60000  69999
    ## 320                                                India          <NA>
    ## 321                                                Japan  40000  49999
    ## 322                                                India   1000   1999
    ## 323                                                India          <NA>
    ## 324                                             Colombia          <NA>
    ## 325                                                India          <NA>
    ## 326                                              Germany          <NA>
    ## 327                                             Cameroon   5000   7499
    ## 328                             United States of America          <NA>
    ## 329                                                Egypt      0    999
    ## 330                                                India          <NA>
    ## 331                         Iran, Islamic Republic of...      0    999
    ## 332                                                India          <NA>
    ## 333                                           Bangladesh          <NA>
    ##  [ reached 'max' / getOption("max.print") -- omitted 23641 rows ]

## Section A

### Part 3. Salary Range Distribution

    usa_sal_range <- global_sal_range %>% filter(Q4 == "United States of America")
    usa_sal_range <- usa_sal_range %>% drop_na()
    #drop na, then convert columns to numeric
    usa_sal_range$MINsal <- as.numeric(usa_sal_range$MINsal)
    usa_sal_range$MAXsal <- as.numeric(usa_sal_range$MAXsal)
    #after, create factor column corresponding to original salary range columns
    usa_sal_range <- usa_sal_range %>% 
      mutate(
        salary_range = paste0 (
      format(MINsal, trim = TRUE),
      "-", 
      MAXsal
    ),
    salary_range = fct_reorder(salary_range, MINsal)
      )

after all that, now we can attempt to visualize

    usa_sal_range

    ##                           Q4 MINsal MAXsal  salary_range
    ## 1   United States of America 200000 249999 200000-249999
    ## 2   United States of America 200000 249999 200000-249999
    ## 3   United States of America 150000 199999 150000-199999
    ## 4   United States of America  25000  29999   25000-29999
    ## 5   United States of America  80000  89999   80000-89999
    ## 6   United States of America 100000 124999 100000-124999
    ## 7   United States of America 250000 299999 250000-299999
    ## 8   United States of America  80000  89999   80000-89999
    ## 9   United States of America  90000  99999   90000-99999
    ## 10  United States of America 150000 199999 150000-199999
    ## 11  United States of America 150000 199999 150000-199999
    ## 12  United States of America      0    999         0-999
    ## 13  United States of America  60000  69999   60000-69999
    ## 14  United States of America 100000 124999 100000-124999
    ## 15  United States of America 125000 149999 125000-149999
    ## 16  United States of America 150000 199999 150000-199999
    ## 17  United States of America 150000 199999 150000-199999
    ## 18  United States of America  90000  99999   90000-99999
    ## 19  United States of America 100000 124999 100000-124999
    ## 20  United States of America 125000 149999 125000-149999
    ## 21  United States of America  80000  89999   80000-89999
    ## 22  United States of America 125000 149999 125000-149999
    ## 23  United States of America   7500   9999     7500-9999
    ## 24  United States of America  80000  89999   80000-89999
    ## 25  United States of America  70000  79999   70000-79999
    ## 26  United States of America 125000 149999 125000-149999
    ## 27  United States of America  20000  24999   20000-24999
    ## 28  United States of America 125000 149999 125000-149999
    ## 29  United States of America 125000 149999 125000-149999
    ## 30  United States of America 125000 149999 125000-149999
    ## 31  United States of America 100000 124999 100000-124999
    ## 32  United States of America 250000 299999 250000-299999
    ## 33  United States of America 100000 124999 100000-124999
    ## 34  United States of America  30000  39999   30000-39999
    ## 35  United States of America  50000  59999   50000-59999
    ## 36  United States of America  80000  89999   80000-89999
    ## 37  United States of America 150000 199999 150000-199999
    ## 38  United States of America  50000  59999   50000-59999
    ## 39  United States of America 125000 149999 125000-149999
    ## 40  United States of America 150000 199999 150000-199999
    ## 41  United States of America 200000 249999 200000-249999
    ## 42  United States of America 100000 124999 100000-124999
    ## 43  United States of America  60000  69999   60000-69999
    ## 44  United States of America 100000 124999 100000-124999
    ## 45  United States of America 150000 199999 150000-199999
    ## 46  United States of America      0    999         0-999
    ## 47  United States of America 200000 249999 200000-249999
    ## 48  United States of America 200000 249999 200000-249999
    ## 49  United States of America  80000  89999   80000-89999
    ## 50  United States of America  90000  99999   90000-99999
    ## 51  United States of America  70000  79999   70000-79999
    ## 52  United States of America 150000 199999 150000-199999
    ## 53  United States of America  90000  99999   90000-99999
    ## 54  United States of America 200000 249999 200000-249999
    ## 55  United States of America 125000 149999 125000-149999
    ## 56  United States of America 200000 249999 200000-249999
    ## 57  United States of America 125000 149999 125000-149999
    ## 58  United States of America   1000   1999     1000-1999
    ## 59  United States of America 150000 199999 150000-199999
    ## 60  United States of America      0    999         0-999
    ## 61  United States of America 150000 199999 150000-199999
    ## 62  United States of America  40000  49999   40000-49999
    ## 63  United States of America   5000   7499     5000-7499
    ## 64  United States of America  60000  69999   60000-69999
    ## 65  United States of America      0    999         0-999
    ## 66  United States of America  80000  89999   80000-89999
    ## 67  United States of America  50000  59999   50000-59999
    ## 68  United States of America 150000 199999 150000-199999
    ## 69  United States of America 200000 249999 200000-249999
    ## 70  United States of America  90000  99999   90000-99999
    ## 71  United States of America 100000 124999 100000-124999
    ## 72  United States of America 100000 124999 100000-124999
    ## 73  United States of America  90000  99999   90000-99999
    ## 74  United States of America 100000 124999 100000-124999
    ## 75  United States of America  70000  79999   70000-79999
    ## 76  United States of America 100000 124999 100000-124999
    ## 77  United States of America  80000  89999   80000-89999
    ## 78  United States of America 100000 124999 100000-124999
    ## 79  United States of America 125000 149999 125000-149999
    ## 80  United States of America  40000  49999   40000-49999
    ## 81  United States of America 100000 124999 100000-124999
    ## 82  United States of America 125000 149999 125000-149999
    ## 83  United States of America  80000  89999   80000-89999
    ## 84  United States of America  50000  59999   50000-59999
    ## 85  United States of America  60000  69999   60000-69999
    ## 86  United States of America  90000  99999   90000-99999
    ## 87  United States of America  50000  59999   50000-59999
    ## 88  United States of America 100000 124999 100000-124999
    ## 89  United States of America 150000 199999 150000-199999
    ## 90  United States of America 150000 199999 150000-199999
    ## 91  United States of America 500000 999999 500000-999999
    ## 92  United States of America  80000  89999   80000-89999
    ## 93  United States of America  90000  99999   90000-99999
    ## 94  United States of America 300000 499999 300000-499999
    ## 95  United States of America 150000 199999 150000-199999
    ## 96  United States of America  70000  79999   70000-79999
    ## 97  United States of America      0    999         0-999
    ## 98  United States of America  90000  99999   90000-99999
    ## 99  United States of America  30000  39999   30000-39999
    ## 100 United States of America  40000  49999   40000-49999
    ## 101 United States of America  70000  79999   70000-79999
    ## 102 United States of America  50000  59999   50000-59999
    ## 103 United States of America 300000 499999 300000-499999
    ## 104 United States of America 100000 124999 100000-124999
    ## 105 United States of America  30000  39999   30000-39999
    ## 106 United States of America  80000  89999   80000-89999
    ## 107 United States of America 150000 199999 150000-199999
    ## 108 United States of America 150000 199999 150000-199999
    ## 109 United States of America 500000 999999 500000-999999
    ## 110 United States of America  80000  89999   80000-89999
    ## 111 United States of America  90000  99999   90000-99999
    ## 112 United States of America 125000 149999 125000-149999
    ## 113 United States of America 150000 199999 150000-199999
    ## 114 United States of America  70000  79999   70000-79999
    ## 115 United States of America 250000 299999 250000-299999
    ## 116 United States of America 500000 999999 500000-999999
    ## 117 United States of America 150000 199999 150000-199999
    ## 118 United States of America 150000 199999 150000-199999
    ## 119 United States of America  80000  89999   80000-89999
    ## 120 United States of America 300000 499999 300000-499999
    ## 121 United States of America  60000  69999   60000-69999
    ## 122 United States of America  90000  99999   90000-99999
    ## 123 United States of America 125000 149999 125000-149999
    ## 124 United States of America 150000 199999 150000-199999
    ## 125 United States of America 150000 199999 150000-199999
    ## 126 United States of America 100000 124999 100000-124999
    ## 127 United States of America 500000 999999 500000-999999
    ## 128 United States of America 250000 299999 250000-299999
    ## 129 United States of America  90000  99999   90000-99999
    ## 130 United States of America  60000  69999   60000-69999
    ## 131 United States of America 125000 149999 125000-149999
    ## 132 United States of America 150000 199999 150000-199999
    ## 133 United States of America 200000 249999 200000-249999
    ## 134 United States of America  20000  24999   20000-24999
    ## 135 United States of America 150000 199999 150000-199999
    ## 136 United States of America 200000 249999 200000-249999
    ## 137 United States of America  90000  99999   90000-99999
    ## 138 United States of America 100000 124999 100000-124999
    ## 139 United States of America 125000 149999 125000-149999
    ## 140 United States of America 200000 249999 200000-249999
    ## 141 United States of America   7500   9999     7500-9999
    ## 142 United States of America 150000 199999 150000-199999
    ## 143 United States of America 250000 299999 250000-299999
    ## 144 United States of America 150000 199999 150000-199999
    ## 145 United States of America   1000   1999     1000-1999
    ## 146 United States of America 125000 149999 125000-149999
    ## 147 United States of America 100000 124999 100000-124999
    ## 148 United States of America 150000 199999 150000-199999
    ## 149 United States of America  50000  59999   50000-59999
    ## 150 United States of America  70000  79999   70000-79999
    ## 151 United States of America  90000  99999   90000-99999
    ## 152 United States of America  80000  89999   80000-89999
    ## 153 United States of America 125000 149999 125000-149999
    ## 154 United States of America 150000 199999 150000-199999
    ## 155 United States of America 250000 299999 250000-299999
    ## 156 United States of America 100000 124999 100000-124999
    ## 157 United States of America 200000 249999 200000-249999
    ## 158 United States of America 125000 149999 125000-149999
    ## 159 United States of America 500000 999999 500000-999999
    ## 160 United States of America 125000 149999 125000-149999
    ## 161 United States of America 150000 199999 150000-199999
    ## 162 United States of America 125000 149999 125000-149999
    ## 163 United States of America  90000  99999   90000-99999
    ## 164 United States of America 100000 124999 100000-124999
    ## 165 United States of America 150000 199999 150000-199999
    ## 166 United States of America      0    999         0-999
    ## 167 United States of America 200000 249999 200000-249999
    ## 168 United States of America 200000 249999 200000-249999
    ## 169 United States of America  80000  89999   80000-89999
    ## 170 United States of America 300000 499999 300000-499999
    ## 171 United States of America 300000 499999 300000-499999
    ## 172 United States of America 200000 249999 200000-249999
    ## 173 United States of America 100000 124999 100000-124999
    ## 174 United States of America 150000 199999 150000-199999
    ## 175 United States of America  50000  59999   50000-59999
    ## 176 United States of America  40000  49999   40000-49999
    ## 177 United States of America  60000  69999   60000-69999
    ## 178 United States of America 300000 499999 300000-499999
    ## 179 United States of America  60000  69999   60000-69999
    ## 180 United States of America 150000 199999 150000-199999
    ## 181 United States of America 500000 999999 500000-999999
    ## 182 United States of America  50000  59999   50000-59999
    ## 183 United States of America 125000 149999 125000-149999
    ## 184 United States of America  50000  59999   50000-59999
    ## 185 United States of America 125000 149999 125000-149999
    ## 186 United States of America 150000 199999 150000-199999
    ## 187 United States of America  90000  99999   90000-99999
    ## 188 United States of America  10000  14999   10000-14999
    ## 189 United States of America 100000 124999 100000-124999
    ## 190 United States of America   3000   3999     3000-3999
    ## 191 United States of America 100000 124999 100000-124999
    ## 192 United States of America  80000  89999   80000-89999
    ## 193 United States of America 150000 199999 150000-199999
    ## 194 United States of America  40000  49999   40000-49999
    ## 195 United States of America 150000 199999 150000-199999
    ## 196 United States of America  70000  79999   70000-79999
    ## 197 United States of America  70000  79999   70000-79999
    ## 198 United States of America 200000 249999 200000-249999
    ## 199 United States of America  90000  99999   90000-99999
    ## 200 United States of America 150000 199999 150000-199999
    ## 201 United States of America 125000 149999 125000-149999
    ## 202 United States of America  40000  49999   40000-49999
    ## 203 United States of America 150000 199999 150000-199999
    ## 204 United States of America 125000 149999 125000-149999
    ## 205 United States of America 100000 124999 100000-124999
    ## 206 United States of America  80000  89999   80000-89999
    ## 207 United States of America 125000 149999 125000-149999
    ## 208 United States of America 150000 199999 150000-199999
    ## 209 United States of America 100000 124999 100000-124999
    ## 210 United States of America  90000  99999   90000-99999
    ## 211 United States of America 250000 299999 250000-299999
    ## 212 United States of America 250000 299999 250000-299999
    ## 213 United States of America  40000  49999   40000-49999
    ## 214 United States of America 100000 124999 100000-124999
    ## 215 United States of America 150000 199999 150000-199999
    ## 216 United States of America 200000 249999 200000-249999
    ## 217 United States of America  90000  99999   90000-99999
    ## 218 United States of America 125000 149999 125000-149999
    ## 219 United States of America 200000 249999 200000-249999
    ## 220 United States of America  80000  89999   80000-89999
    ## 221 United States of America 150000 199999 150000-199999
    ## 222 United States of America 125000 149999 125000-149999
    ## 223 United States of America 125000 149999 125000-149999
    ## 224 United States of America      0    999         0-999
    ## 225 United States of America  80000  89999   80000-89999
    ## 226 United States of America 200000 249999 200000-249999
    ## 227 United States of America 100000 124999 100000-124999
    ## 228 United States of America  15000  19999   15000-19999
    ## 229 United States of America 150000 199999 150000-199999
    ## 230 United States of America 250000 299999 250000-299999
    ## 231 United States of America 300000 499999 300000-499999
    ## 232 United States of America 125000 149999 125000-149999
    ## 233 United States of America 125000 149999 125000-149999
    ## 234 United States of America  50000  59999   50000-59999
    ## 235 United States of America 250000 299999 250000-299999
    ## 236 United States of America 150000 199999 150000-199999
    ## 237 United States of America 300000 499999 300000-499999
    ## 238 United States of America 250000 299999 250000-299999
    ## 239 United States of America 250000 299999 250000-299999
    ## 240 United States of America  70000  79999   70000-79999
    ## 241 United States of America 300000 499999 300000-499999
    ## 242 United States of America 150000 199999 150000-199999
    ## 243 United States of America  80000  89999   80000-89999
    ## 244 United States of America      0    999         0-999
    ## 245 United States of America 125000 149999 125000-149999
    ## 246 United States of America 200000 249999 200000-249999
    ## 247 United States of America 125000 149999 125000-149999
    ## 248 United States of America  90000  99999   90000-99999
    ## 249 United States of America  80000  89999   80000-89999
    ## 250 United States of America 100000 124999 100000-124999
    ##  [ reached 'max' / getOption("max.print") -- omitted 1176 rows ]

    usa_salary <- usa_sal_range %>% select(salary_range) %>% count(salary_range)

    ggplot(usa_salary, aes(x=salary_range, y=n)) +

      geom_col(fill="skyblue") +
      
      coord_flip() + theme_minimal() + labs(x="salary range, annual (USD)", y="count")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-61-1.png)

    usa_salary

    ##     salary_range   n
    ## 1          0-999  56
    ## 2      1000-1999   3
    ## 3      2000-2999   3
    ## 4      3000-3999   3
    ## 5      4000-4999   1
    ## 6      5000-7499   5
    ## 7      7500-9999   8
    ## 8    10000-14999  11
    ## 9    15000-19999   6
    ## 10   20000-24999  13
    ## 11   25000-29999  10
    ## 12   30000-39999  36
    ## 13   40000-49999  47
    ## 14   50000-59999  50
    ## 15   60000-69999  71
    ## 16   70000-79999  76
    ## 17   80000-89999  68
    ## 18   90000-99999  81
    ## 19 100000-124999 176
    ## 20 125000-149999 185
    ## 21 150000-199999 263
    ## 22 200000-249999 117
    ## 23 250000-299999  56
    ## 24 300000-499999  51
    ## 25 500000-999999  30

    #filter for non-USA countries, then drop_na
    not_usa_salary <- global_sal_range %>% filter(Q4 != "United States of America")
    not_usa_salary <- not_usa_salary %>% drop_na()

    #convert to numeric dtype
    not_usa_salary$MINsal <- as.numeric(not_usa_salary$MINsal)
    not_usa_salary$MAXsal <- as.numeric(not_usa_salary$MAXsal)
    #after, create factor column corresponding to original salary range columns
    not_usa_salary <- not_usa_salary %>% 
      mutate(
        salary_range = paste0 (
      format(MINsal, trim = TRUE),
      "-", 
      MAXsal
    ),
    salary_range = fct_reorder(salary_range, MINsal)
      )

    #convert values to "rest of world"
    not_usa_salary['Q4'] <- "Rest of World"

    #combine the two dataframes by row (not by column)
    usa_v_row_sal <- rbind(usa_sal_range, not_usa_salary)

    usa_v_row_sal <- usa_v_row_sal %>% rename("country" = "Q4")

    usa_v_row_sal

    ##                      country MINsal MAXsal  salary_range
    ## 1   United States of America 200000 249999 200000-249999
    ## 2   United States of America 200000 249999 200000-249999
    ## 3   United States of America 150000 199999 150000-199999
    ## 4   United States of America  25000  29999   25000-29999
    ## 5   United States of America  80000  89999   80000-89999
    ## 6   United States of America 100000 124999 100000-124999
    ## 7   United States of America 250000 299999 250000-299999
    ## 8   United States of America  80000  89999   80000-89999
    ## 9   United States of America  90000  99999   90000-99999
    ## 10  United States of America 150000 199999 150000-199999
    ## 11  United States of America 150000 199999 150000-199999
    ## 12  United States of America      0    999         0-999
    ## 13  United States of America  60000  69999   60000-69999
    ## 14  United States of America 100000 124999 100000-124999
    ## 15  United States of America 125000 149999 125000-149999
    ## 16  United States of America 150000 199999 150000-199999
    ## 17  United States of America 150000 199999 150000-199999
    ## 18  United States of America  90000  99999   90000-99999
    ## 19  United States of America 100000 124999 100000-124999
    ## 20  United States of America 125000 149999 125000-149999
    ## 21  United States of America  80000  89999   80000-89999
    ## 22  United States of America 125000 149999 125000-149999
    ## 23  United States of America   7500   9999     7500-9999
    ## 24  United States of America  80000  89999   80000-89999
    ## 25  United States of America  70000  79999   70000-79999
    ## 26  United States of America 125000 149999 125000-149999
    ## 27  United States of America  20000  24999   20000-24999
    ## 28  United States of America 125000 149999 125000-149999
    ## 29  United States of America 125000 149999 125000-149999
    ## 30  United States of America 125000 149999 125000-149999
    ## 31  United States of America 100000 124999 100000-124999
    ## 32  United States of America 250000 299999 250000-299999
    ## 33  United States of America 100000 124999 100000-124999
    ## 34  United States of America  30000  39999   30000-39999
    ## 35  United States of America  50000  59999   50000-59999
    ## 36  United States of America  80000  89999   80000-89999
    ## 37  United States of America 150000 199999 150000-199999
    ## 38  United States of America  50000  59999   50000-59999
    ## 39  United States of America 125000 149999 125000-149999
    ## 40  United States of America 150000 199999 150000-199999
    ## 41  United States of America 200000 249999 200000-249999
    ## 42  United States of America 100000 124999 100000-124999
    ## 43  United States of America  60000  69999   60000-69999
    ## 44  United States of America 100000 124999 100000-124999
    ## 45  United States of America 150000 199999 150000-199999
    ## 46  United States of America      0    999         0-999
    ## 47  United States of America 200000 249999 200000-249999
    ## 48  United States of America 200000 249999 200000-249999
    ## 49  United States of America  80000  89999   80000-89999
    ## 50  United States of America  90000  99999   90000-99999
    ## 51  United States of America  70000  79999   70000-79999
    ## 52  United States of America 150000 199999 150000-199999
    ## 53  United States of America  90000  99999   90000-99999
    ## 54  United States of America 200000 249999 200000-249999
    ## 55  United States of America 125000 149999 125000-149999
    ## 56  United States of America 200000 249999 200000-249999
    ## 57  United States of America 125000 149999 125000-149999
    ## 58  United States of America   1000   1999     1000-1999
    ## 59  United States of America 150000 199999 150000-199999
    ## 60  United States of America      0    999         0-999
    ## 61  United States of America 150000 199999 150000-199999
    ## 62  United States of America  40000  49999   40000-49999
    ## 63  United States of America   5000   7499     5000-7499
    ## 64  United States of America  60000  69999   60000-69999
    ## 65  United States of America      0    999         0-999
    ## 66  United States of America  80000  89999   80000-89999
    ## 67  United States of America  50000  59999   50000-59999
    ## 68  United States of America 150000 199999 150000-199999
    ## 69  United States of America 200000 249999 200000-249999
    ## 70  United States of America  90000  99999   90000-99999
    ## 71  United States of America 100000 124999 100000-124999
    ## 72  United States of America 100000 124999 100000-124999
    ## 73  United States of America  90000  99999   90000-99999
    ## 74  United States of America 100000 124999 100000-124999
    ## 75  United States of America  70000  79999   70000-79999
    ## 76  United States of America 100000 124999 100000-124999
    ## 77  United States of America  80000  89999   80000-89999
    ## 78  United States of America 100000 124999 100000-124999
    ## 79  United States of America 125000 149999 125000-149999
    ## 80  United States of America  40000  49999   40000-49999
    ## 81  United States of America 100000 124999 100000-124999
    ## 82  United States of America 125000 149999 125000-149999
    ## 83  United States of America  80000  89999   80000-89999
    ## 84  United States of America  50000  59999   50000-59999
    ## 85  United States of America  60000  69999   60000-69999
    ## 86  United States of America  90000  99999   90000-99999
    ## 87  United States of America  50000  59999   50000-59999
    ## 88  United States of America 100000 124999 100000-124999
    ## 89  United States of America 150000 199999 150000-199999
    ## 90  United States of America 150000 199999 150000-199999
    ## 91  United States of America 500000 999999 500000-999999
    ## 92  United States of America  80000  89999   80000-89999
    ## 93  United States of America  90000  99999   90000-99999
    ## 94  United States of America 300000 499999 300000-499999
    ## 95  United States of America 150000 199999 150000-199999
    ## 96  United States of America  70000  79999   70000-79999
    ## 97  United States of America      0    999         0-999
    ## 98  United States of America  90000  99999   90000-99999
    ## 99  United States of America  30000  39999   30000-39999
    ## 100 United States of America  40000  49999   40000-49999
    ## 101 United States of America  70000  79999   70000-79999
    ## 102 United States of America  50000  59999   50000-59999
    ## 103 United States of America 300000 499999 300000-499999
    ## 104 United States of America 100000 124999 100000-124999
    ## 105 United States of America  30000  39999   30000-39999
    ## 106 United States of America  80000  89999   80000-89999
    ## 107 United States of America 150000 199999 150000-199999
    ## 108 United States of America 150000 199999 150000-199999
    ## 109 United States of America 500000 999999 500000-999999
    ## 110 United States of America  80000  89999   80000-89999
    ## 111 United States of America  90000  99999   90000-99999
    ## 112 United States of America 125000 149999 125000-149999
    ## 113 United States of America 150000 199999 150000-199999
    ## 114 United States of America  70000  79999   70000-79999
    ## 115 United States of America 250000 299999 250000-299999
    ## 116 United States of America 500000 999999 500000-999999
    ## 117 United States of America 150000 199999 150000-199999
    ## 118 United States of America 150000 199999 150000-199999
    ## 119 United States of America  80000  89999   80000-89999
    ## 120 United States of America 300000 499999 300000-499999
    ## 121 United States of America  60000  69999   60000-69999
    ## 122 United States of America  90000  99999   90000-99999
    ## 123 United States of America 125000 149999 125000-149999
    ## 124 United States of America 150000 199999 150000-199999
    ## 125 United States of America 150000 199999 150000-199999
    ## 126 United States of America 100000 124999 100000-124999
    ## 127 United States of America 500000 999999 500000-999999
    ## 128 United States of America 250000 299999 250000-299999
    ## 129 United States of America  90000  99999   90000-99999
    ## 130 United States of America  60000  69999   60000-69999
    ## 131 United States of America 125000 149999 125000-149999
    ## 132 United States of America 150000 199999 150000-199999
    ## 133 United States of America 200000 249999 200000-249999
    ## 134 United States of America  20000  24999   20000-24999
    ## 135 United States of America 150000 199999 150000-199999
    ## 136 United States of America 200000 249999 200000-249999
    ## 137 United States of America  90000  99999   90000-99999
    ## 138 United States of America 100000 124999 100000-124999
    ## 139 United States of America 125000 149999 125000-149999
    ## 140 United States of America 200000 249999 200000-249999
    ## 141 United States of America   7500   9999     7500-9999
    ## 142 United States of America 150000 199999 150000-199999
    ## 143 United States of America 250000 299999 250000-299999
    ## 144 United States of America 150000 199999 150000-199999
    ## 145 United States of America   1000   1999     1000-1999
    ## 146 United States of America 125000 149999 125000-149999
    ## 147 United States of America 100000 124999 100000-124999
    ## 148 United States of America 150000 199999 150000-199999
    ## 149 United States of America  50000  59999   50000-59999
    ## 150 United States of America  70000  79999   70000-79999
    ## 151 United States of America  90000  99999   90000-99999
    ## 152 United States of America  80000  89999   80000-89999
    ## 153 United States of America 125000 149999 125000-149999
    ## 154 United States of America 150000 199999 150000-199999
    ## 155 United States of America 250000 299999 250000-299999
    ## 156 United States of America 100000 124999 100000-124999
    ## 157 United States of America 200000 249999 200000-249999
    ## 158 United States of America 125000 149999 125000-149999
    ## 159 United States of America 500000 999999 500000-999999
    ## 160 United States of America 125000 149999 125000-149999
    ## 161 United States of America 150000 199999 150000-199999
    ## 162 United States of America 125000 149999 125000-149999
    ## 163 United States of America  90000  99999   90000-99999
    ## 164 United States of America 100000 124999 100000-124999
    ## 165 United States of America 150000 199999 150000-199999
    ## 166 United States of America      0    999         0-999
    ## 167 United States of America 200000 249999 200000-249999
    ## 168 United States of America 200000 249999 200000-249999
    ## 169 United States of America  80000  89999   80000-89999
    ## 170 United States of America 300000 499999 300000-499999
    ## 171 United States of America 300000 499999 300000-499999
    ## 172 United States of America 200000 249999 200000-249999
    ## 173 United States of America 100000 124999 100000-124999
    ## 174 United States of America 150000 199999 150000-199999
    ## 175 United States of America  50000  59999   50000-59999
    ## 176 United States of America  40000  49999   40000-49999
    ## 177 United States of America  60000  69999   60000-69999
    ## 178 United States of America 300000 499999 300000-499999
    ## 179 United States of America  60000  69999   60000-69999
    ## 180 United States of America 150000 199999 150000-199999
    ## 181 United States of America 500000 999999 500000-999999
    ## 182 United States of America  50000  59999   50000-59999
    ## 183 United States of America 125000 149999 125000-149999
    ## 184 United States of America  50000  59999   50000-59999
    ## 185 United States of America 125000 149999 125000-149999
    ## 186 United States of America 150000 199999 150000-199999
    ## 187 United States of America  90000  99999   90000-99999
    ## 188 United States of America  10000  14999   10000-14999
    ## 189 United States of America 100000 124999 100000-124999
    ## 190 United States of America   3000   3999     3000-3999
    ## 191 United States of America 100000 124999 100000-124999
    ## 192 United States of America  80000  89999   80000-89999
    ## 193 United States of America 150000 199999 150000-199999
    ## 194 United States of America  40000  49999   40000-49999
    ## 195 United States of America 150000 199999 150000-199999
    ## 196 United States of America  70000  79999   70000-79999
    ## 197 United States of America  70000  79999   70000-79999
    ## 198 United States of America 200000 249999 200000-249999
    ## 199 United States of America  90000  99999   90000-99999
    ## 200 United States of America 150000 199999 150000-199999
    ## 201 United States of America 125000 149999 125000-149999
    ## 202 United States of America  40000  49999   40000-49999
    ## 203 United States of America 150000 199999 150000-199999
    ## 204 United States of America 125000 149999 125000-149999
    ## 205 United States of America 100000 124999 100000-124999
    ## 206 United States of America  80000  89999   80000-89999
    ## 207 United States of America 125000 149999 125000-149999
    ## 208 United States of America 150000 199999 150000-199999
    ## 209 United States of America 100000 124999 100000-124999
    ## 210 United States of America  90000  99999   90000-99999
    ## 211 United States of America 250000 299999 250000-299999
    ## 212 United States of America 250000 299999 250000-299999
    ## 213 United States of America  40000  49999   40000-49999
    ## 214 United States of America 100000 124999 100000-124999
    ## 215 United States of America 150000 199999 150000-199999
    ## 216 United States of America 200000 249999 200000-249999
    ## 217 United States of America  90000  99999   90000-99999
    ## 218 United States of America 125000 149999 125000-149999
    ## 219 United States of America 200000 249999 200000-249999
    ## 220 United States of America  80000  89999   80000-89999
    ## 221 United States of America 150000 199999 150000-199999
    ## 222 United States of America 125000 149999 125000-149999
    ## 223 United States of America 125000 149999 125000-149999
    ## 224 United States of America      0    999         0-999
    ## 225 United States of America  80000  89999   80000-89999
    ## 226 United States of America 200000 249999 200000-249999
    ## 227 United States of America 100000 124999 100000-124999
    ## 228 United States of America  15000  19999   15000-19999
    ## 229 United States of America 150000 199999 150000-199999
    ## 230 United States of America 250000 299999 250000-299999
    ## 231 United States of America 300000 499999 300000-499999
    ## 232 United States of America 125000 149999 125000-149999
    ## 233 United States of America 125000 149999 125000-149999
    ## 234 United States of America  50000  59999   50000-59999
    ## 235 United States of America 250000 299999 250000-299999
    ## 236 United States of America 150000 199999 150000-199999
    ## 237 United States of America 300000 499999 300000-499999
    ## 238 United States of America 250000 299999 250000-299999
    ## 239 United States of America 250000 299999 250000-299999
    ## 240 United States of America  70000  79999   70000-79999
    ## 241 United States of America 300000 499999 300000-499999
    ## 242 United States of America 150000 199999 150000-199999
    ## 243 United States of America  80000  89999   80000-89999
    ## 244 United States of America      0    999         0-999
    ## 245 United States of America 125000 149999 125000-149999
    ## 246 United States of America 200000 249999 200000-249999
    ## 247 United States of America 125000 149999 125000-149999
    ## 248 United States of America  90000  99999   90000-99999
    ## 249 United States of America  80000  89999   80000-89999
    ## 250 United States of America 100000 124999 100000-124999
    ##  [ reached 'max' / getOption("max.print") -- omitted 7863 rows ]

attempt to plot histogram with USA salary range vs Rest of World

    ggplot(usa_v_row_sal, aes(x=salary_range, fill=country, color=country)) +
      
      geom_bar(color="white", width=1, linewidth=0) + coord_flip() + theme_minimal() +
      
      scale_fill_manual(values=c("grey90","skyblue")) + 
      
      labs(x="salary range, annual (USD)", y="count") +
      theme(panel.grid.major = element_blank(),
            panel.grid.minor = element_blank(),
            legend.position = c(0.87, 0.25))

![](finalproj_files/figure-markdown_strict/unnamed-chunk-67-1.png)

    ggsave("salary_distribution.png")

    ## Saving 8 x 6 in image

## Section B. Job Dynamics

### Part 1.Salary Range Density, by Job title, by Gender

    gender_job

    ##     gender                        job_title
    ## 1      Man                   Data Scientist
    ## 2      Man                Software Engineer
    ## 3      Man               Research Scientist
    ## 4      Man               Developer Advocate
    ## 5      Man                   Data Scientist
    ## 6      Man                   Data Scientist
    ## 7      Man                     Data Analyst
    ## 8      Man                    Data Engineer
    ## 9      Man                     Data Analyst
    ## 10     Man                    Data Engineer
    ## 11     Man                   Data Scientist
    ## 12     Man Machine Learning/ MLops Engineer
    ## 13     Man               Research Scientist
    ## 14     Man                   Data Scientist
    ## 15   Woman Machine Learning/ MLops Engineer
    ## 16     Man                   Data Scientist
    ## 17     Man Machine Learning/ MLops Engineer
    ## 18     Man          Engineer (non-software)
    ## 19     Man               Research Scientist
    ## 20     Man                   Data Scientist
    ## 21     Man                     Data Analyst
    ## 22     Man                Software Engineer
    ## 23     Man                     Data Analyst
    ## 24     Man                     Data Analyst
    ## 25     Man          Engineer (non-software)
    ## 26     Man                Software Engineer
    ## 27   Woman              Teacher / professor
    ## 28     Man                Software Engineer
    ## 29     Man              Teacher / professor
    ## 30     Man                     Data Analyst
    ## 31     Man                Software Engineer
    ## 32     Man                     Data Analyst
    ## 33   Woman                   Data Scientist
    ## 34     Man                   Data Scientist
    ## 35     Man                   Data Scientist
    ## 36     Man                   Data Scientist
    ## 37   Woman                Software Engineer
    ## 38     Man                     Data Analyst
    ## 39     Man                   Data Scientist
    ## 40     Man               Research Scientist
    ## 41     Man                   Data Scientist
    ## 42     Man                   Data Scientist
    ## 43     Man          Engineer (non-software)
    ## 44     Man                Software Engineer
    ## 45     Man                     Data Analyst
    ## 46     Man                     Data Analyst
    ## 47     Man                     Statistician
    ## 48     Man                   Data Scientist
    ## 49   Woman              Teacher / professor
    ## 50   Woman                     Data Analyst
    ## 51     Man                Software Engineer
    ## 52     Man                   Data Scientist
    ## 53     Man Machine Learning/ MLops Engineer
    ## 54     Man                    Data Engineer
    ## 55     Man                Software Engineer
    ## 56     Man              Teacher / professor
    ## 57   Woman                     Data Analyst
    ## 58     Man                    Data Engineer
    ## 59     Man                     Data Analyst
    ## 60     Man                   Data Scientist
    ## 61     Man              Teacher / professor
    ## 62     Man                Software Engineer
    ## 63     Man                          Manager
    ## 64     Man                          Manager
    ## 65     Man                   Data Scientist
    ## 66     Man                   Data Scientist
    ## 67     Man              Teacher / professor
    ## 68     Man                   Data Scientist
    ## 69     Man                    Data Engineer
    ## 70     Man              Teacher / professor
    ## 71   Woman                          Manager
    ## 72     Man          Engineer (non-software)
    ## 73     Man                          Manager
    ## 74     Man                          Manager
    ## 75     Man                    Data Engineer
    ## 76   Woman              Teacher / professor
    ## 77     Man                          Manager
    ## 78     Man                   Data Scientist
    ## 79   Woman                     Statistician
    ## 80   Woman                   Data Scientist
    ## 81     Man                   Data Scientist
    ## 82   Woman               Data Administrator
    ## 83     Man          Engineer (non-software)
    ## 84     Man                Software Engineer
    ## 85     Man                   Data Scientist
    ## 86     Man                   Data Scientist
    ## 87     Man              Teacher / professor
    ## 88     Man                     Data Analyst
    ## 89     Man                          Manager
    ## 90   Woman                     Data Analyst
    ## 91     Man                Software Engineer
    ## 92     Man                Software Engineer
    ## 93     Man                   Data Scientist
    ## 94     Man                     Data Analyst
    ## 95     Man          Engineer (non-software)
    ## 96   Woman               Research Scientist
    ## 97   Woman                   Data Scientist
    ## 98     Man              Teacher / professor
    ## 99     Man                   Data Scientist
    ## 100    Man                     Data Analyst
    ## 101    Man          Engineer (non-software)
    ## 102    Man                    Data Engineer
    ## 103    Man                Software Engineer
    ## 104    Man              Teacher / professor
    ## 105  Woman                   Data Scientist
    ## 106  Woman               Research Scientist
    ## 107  Woman               Research Scientist
    ## 108    Man Machine Learning/ MLops Engineer
    ## 109    Man                   Data Scientist
    ## 110    Man                Software Engineer
    ## 111    Man                Software Engineer
    ## 112    Man                     Data Analyst
    ## 113    Man                          Manager
    ## 114    Man                     Data Analyst
    ## 115    Man                     Data Analyst
    ## 116    Man                     Statistician
    ## 117    Man                Software Engineer
    ## 118    Man               Research Scientist
    ## 119    Man                          Manager
    ## 120    Man                   Data Scientist
    ## 121    Man                Software Engineer
    ## 122    Man                   Data Scientist
    ## 123    Man                          Manager
    ## 124    Man Machine Learning/ MLops Engineer
    ## 125    Man                   Data Scientist
    ## 126  Woman Machine Learning/ MLops Engineer
    ## 127    Man                    Data Engineer
    ## 128  Woman                    Data Engineer
    ## 129    Man                Software Engineer
    ## 130    Man              Teacher / professor
    ## 131  Woman                     Data Analyst
    ## 132  Woman                          Manager
    ## 133    Man                   Data Scientist
    ## 134    Man               Research Scientist
    ## 135    Man                   Data Scientist
    ## 136    Man                     Data Analyst
    ## 137    Man              Teacher / professor
    ## 138    Man Machine Learning/ MLops Engineer
    ## 139    Man                          Manager
    ## 140    Man               Research Scientist
    ## 141  Woman                   Data Scientist
    ## 142    Man Machine Learning/ MLops Engineer
    ## 143  Woman              Teacher / professor
    ## 144  Woman               Research Scientist
    ## 145    Man                    Data Engineer
    ## 146    Man              Teacher / professor
    ## 147  Woman                     Data Analyst
    ## 148    Man          Engineer (non-software)
    ## 149    Man                   Data Scientist
    ## 150    Man                     Data Analyst
    ## 151    Man                          Manager
    ## 152    Man                   Data Architect
    ## 153    Man Machine Learning/ MLops Engineer
    ## 154    Man                          Manager
    ## 155    Man          Engineer (non-software)
    ## 156    Man                Software Engineer
    ## 157    Man                          Manager
    ## 158    Man                     Data Analyst
    ## 159  Woman                Software Engineer
    ## 160    Man                          Manager
    ## 161    Man              Teacher / professor
    ## 162    Man Machine Learning/ MLops Engineer
    ## 163    Man                     Data Analyst
    ## 164    Man Machine Learning/ MLops Engineer
    ## 165    Man                          Manager
    ## 166    Man                   Data Scientist
    ## 167    Man                   Data Scientist
    ## 168    Man          Engineer (non-software)
    ## 169    Man                     Data Analyst
    ## 170    Man                   Data Scientist
    ## 171    Man                     Data Analyst
    ## 172    Man          Engineer (non-software)
    ## 173    Man                   Data Scientist
    ## 174  Woman                   Data Scientist
    ## 175    Man               Research Scientist
    ## 176    Man                     Data Analyst
    ## 177    Man                          Manager
    ## 178    Man                     Data Analyst
    ## 179    Man                Software Engineer
    ## 180    Man                     Data Analyst
    ## 181    Man                   Data Scientist
    ## 182    Man                   Data Scientist
    ## 183    Man                Software Engineer
    ## 184  Woman               Research Scientist
    ## 185    Man                          Manager
    ## 186    Man Machine Learning/ MLops Engineer
    ## 187    Man                   Data Scientist
    ## 188    Man                          Manager
    ## 189    Man                Software Engineer
    ## 190    Man          Engineer (non-software)
    ## 191    Man                     Data Analyst
    ## 192    Man                   Data Scientist
    ## 193  Woman          Engineer (non-software)
    ## 194  Woman                Software Engineer
    ## 195    Man                     Data Analyst
    ## 196    Man                    Data Engineer
    ## 197    Man                     Statistician
    ## 198    Man                Software Engineer
    ## 199    Man              Teacher / professor
    ## 200    Man                     Data Analyst
    ## 201    Man              Teacher / professor
    ## 202    Man                   Data Scientist
    ## 203    Man                Software Engineer
    ## 204    Man              Teacher / professor
    ## 205    Man                    Data Engineer
    ## 206    Man                     Data Analyst
    ## 207    Man          Engineer (non-software)
    ## 208    Man                Software Engineer
    ## 209    Man                Software Engineer
    ## 210    Man Machine Learning/ MLops Engineer
    ## 211  Woman                          Manager
    ## 212    Man                Software Engineer
    ## 213    Man                Software Engineer
    ## 214    Man                   Data Scientist
    ## 215  Woman                   Data Architect
    ## 216    Man                Software Engineer
    ## 217    Man                Software Engineer
    ## 218    Man                   Data Scientist
    ## 219    Man                   Data Scientist
    ## 220    Man Machine Learning/ MLops Engineer
    ## 221    Man                Software Engineer
    ## 222  Woman                   Data Scientist
    ## 223    Man                Software Engineer
    ## 224    Man              Teacher / professor
    ## 225    Man                     Statistician
    ## 226    Man              Teacher / professor
    ## 227    Man                   Data Scientist
    ## 228    Man                   Data Scientist
    ## 229  Woman              Teacher / professor
    ## 230    Man                   Data Scientist
    ## 231    Man                     Data Analyst
    ## 232    Man          Engineer (non-software)
    ## 233  Woman                     Data Analyst
    ## 234    Man               Developer Advocate
    ## 235  Woman                     Data Analyst
    ## 236  Woman                   Data Scientist
    ## 237    Man              Teacher / professor
    ## 238  Woman                     Statistician
    ## 239    Man                     Data Analyst
    ## 240    Man               Research Scientist
    ## 241    Man          Engineer (non-software)
    ## 242    Man Machine Learning/ MLops Engineer
    ## 243    Man               Research Scientist
    ## 244    Man              Teacher / professor
    ## 245    Man          Engineer (non-software)
    ## 246    Man                   Data Scientist
    ## 247  Woman                   Data Scientist
    ## 248    Man          Engineer (non-software)
    ## 249    Man                   Data Scientist
    ## 250    Man               Research Scientist
    ## 251    Man                   Data Scientist
    ## 252    Man                     Statistician
    ## 253    Man                   Data Scientist
    ## 254    Man                          Manager
    ## 255  Woman                     Data Analyst
    ## 256    Man Machine Learning/ MLops Engineer
    ## 257    Man                          Manager
    ## 258    Man                   Data Scientist
    ## 259    Man                     Data Analyst
    ## 260    Man                Software Engineer
    ## 261    Man                   Data Scientist
    ## 262    Man               Research Scientist
    ## 263    Man                   Data Scientist
    ## 264  Woman               Research Scientist
    ## 265    Man                Software Engineer
    ## 266    Man                    Data Engineer
    ## 267    Man                     Data Analyst
    ## 268    Man                   Data Scientist
    ## 269    Man                   Data Scientist
    ## 270    Man              Teacher / professor
    ## 271    Man                   Data Scientist
    ## 272    Man          Engineer (non-software)
    ## 273    Man               Research Scientist
    ## 274    Man                   Data Scientist
    ## 275    Man                     Data Analyst
    ## 276    Man                   Data Scientist
    ## 277    Man                Software Engineer
    ## 278    Man Machine Learning/ MLops Engineer
    ## 279    Man                Software Engineer
    ## 280    Man                          Manager
    ## 281    Man          Engineer (non-software)
    ## 282    Man               Research Scientist
    ## 283    Man                Software Engineer
    ## 284    Man          Engineer (non-software)
    ## 285    Man                     Data Analyst
    ## 286    Man                   Data Scientist
    ## 287    Man                   Data Scientist
    ## 288  Woman                     Data Analyst
    ## 289    Man               Research Scientist
    ## 290    Man Machine Learning/ MLops Engineer
    ## 291    Man                Software Engineer
    ## 292    Man               Research Scientist
    ## 293    Man                    Data Engineer
    ## 294  Woman                          Manager
    ## 295    Man Machine Learning/ MLops Engineer
    ## 296    Man          Engineer (non-software)
    ## 297    Man                   Data Scientist
    ## 298    Man Machine Learning/ MLops Engineer
    ## 299    Man              Teacher / professor
    ## 300    Man                Software Engineer
    ## 301    Man                   Data Architect
    ## 302    Man                   Data Scientist
    ## 303    Man          Engineer (non-software)
    ## 304    Man                Software Engineer
    ## 305  Woman                     Data Analyst
    ## 306    Man                Software Engineer
    ## 307    Man                   Data Architect
    ## 308  Woman              Teacher / professor
    ## 309    Man                     Data Analyst
    ## 310  Woman                Software Engineer
    ## 311    Man              Teacher / professor
    ## 312    Man                   Data Architect
    ## 313    Man                   Data Scientist
    ## 314    Man                     Data Analyst
    ## 315    Man                     Data Analyst
    ## 316  Woman                   Data Scientist
    ## 317  Woman              Teacher / professor
    ## 318  Woman               Research Scientist
    ## 319    Man                     Data Analyst
    ## 320  Woman                     Data Analyst
    ## 321    Man                Software Engineer
    ## 322    Man                   Data Scientist
    ## 323  Woman                          Manager
    ## 324    Man                   Data Architect
    ## 325    Man                Software Engineer
    ## 326    Man                Software Engineer
    ## 327  Woman Machine Learning/ MLops Engineer
    ## 328  Woman              Teacher / professor
    ## 329    Man              Teacher / professor
    ## 330    Man                   Data Scientist
    ## 331    Man                     Data Analyst
    ## 332    Man               Research Scientist
    ## 333    Man                     Data Analyst
    ## 334    Man                          Manager
    ## 335    Man                          Manager
    ## 336  Woman              Teacher / professor
    ## 337    Man                   Data Scientist
    ## 338    Man               Research Scientist
    ## 339    Man                     Data Analyst
    ## 340    Man                Software Engineer
    ## 341  Woman          Engineer (non-software)
    ## 342    Man                     Data Analyst
    ## 343    Man                     Data Analyst
    ## 344  Woman                   Data Scientist
    ## 345    Man                          Manager
    ## 346    Man                     Data Analyst
    ## 347    Man                     Data Analyst
    ## 348    Man              Teacher / professor
    ## 349    Man          Engineer (non-software)
    ## 350  Woman               Research Scientist
    ## 351    Man                          Manager
    ## 352    Man                Software Engineer
    ## 353  Woman          Engineer (non-software)
    ## 354    Man                     Data Analyst
    ## 355    Man                   Data Scientist
    ## 356    Man                   Data Scientist
    ## 357    Man                Software Engineer
    ## 358    Man                   Data Scientist
    ## 359    Man                   Data Scientist
    ## 360    Man               Data Administrator
    ## 361    Man                   Data Scientist
    ## 362    Man                Software Engineer
    ## 363    Man          Engineer (non-software)
    ## 364    Man Machine Learning/ MLops Engineer
    ## 365  Woman          Engineer (non-software)
    ## 366    Man                Software Engineer
    ## 367    Man                   Data Scientist
    ## 368  Woman                Software Engineer
    ## 369    Man                     Data Analyst
    ## 370    Man                     Data Analyst
    ## 371  Woman              Teacher / professor
    ## 372    Man                     Data Analyst
    ## 373    Man                          Manager
    ## 374    Man                          Manager
    ## 375    Man                     Data Analyst
    ## 376    Man              Teacher / professor
    ## 377  Woman                          Manager
    ## 378    Man                     Data Analyst
    ## 379    Man                     Data Analyst
    ## 380    Man               Research Scientist
    ## 381    Man              Teacher / professor
    ## 382    Man              Teacher / professor
    ## 383  Woman                          Manager
    ## 384    Man              Teacher / professor
    ## 385    Man                   Data Scientist
    ## 386    Man                     Data Analyst
    ## 387    Man                     Data Analyst
    ## 388    Man                   Data Scientist
    ## 389    Man                   Data Scientist
    ## 390    Man              Teacher / professor
    ## 391  Woman                Software Engineer
    ## 392    Man                     Data Analyst
    ## 393    Man                   Data Scientist
    ## 394    Man                   Data Scientist
    ## 395    Man                   Data Scientist
    ## 396    Man              Teacher / professor
    ## 397    Man                Software Engineer
    ## 398    Man                   Data Scientist
    ## 399    Man                   Data Scientist
    ## 400    Man               Data Administrator
    ## 401  Woman                Software Engineer
    ## 402    Man                          Manager
    ## 403  Woman                     Data Analyst
    ## 404    Man                   Data Scientist
    ## 405    Man                   Data Scientist
    ## 406    Man                          Manager
    ## 407    Man                Software Engineer
    ## 408    Man              Teacher / professor
    ## 409    Man Machine Learning/ MLops Engineer
    ## 410    Man                          Manager
    ## 411    Man                   Data Scientist
    ## 412    Man          Engineer (non-software)
    ## 413    Man                   Data Scientist
    ## 414    Man                   Data Scientist
    ## 415    Man                   Data Scientist
    ## 416    Man                   Data Scientist
    ## 417    Man                     Data Analyst
    ## 418    Man              Teacher / professor
    ## 419    Man                    Data Engineer
    ## 420    Man                          Manager
    ## 421    Man                          Manager
    ## 422  Woman                Software Engineer
    ## 423  Woman              Teacher / professor
    ## 424    Man              Teacher / professor
    ## 425    Man                          Manager
    ## 426    Man                     Data Analyst
    ## 427    Man                   Data Scientist
    ## 428    Man                   Data Scientist
    ## 429  Woman          Engineer (non-software)
    ## 430  Woman                   Data Scientist
    ## 431    Man                   Data Scientist
    ## 432    Man                Software Engineer
    ## 433    Man                     Data Analyst
    ## 434    Man                          Manager
    ## 435    Man               Research Scientist
    ## 436    Man                    Data Engineer
    ## 437    Man                Software Engineer
    ## 438    Man                          Manager
    ## 439    Man Machine Learning/ MLops Engineer
    ## 440    Man                   Data Scientist
    ## 441    Man                          Manager
    ## 442    Man                          Manager
    ## 443    Man                Software Engineer
    ## 444    Man                   Data Scientist
    ## 445  Woman              Teacher / professor
    ## 446    Man                     Data Analyst
    ## 447  Woman                     Data Analyst
    ## 448    Man          Engineer (non-software)
    ## 449    Man Machine Learning/ MLops Engineer
    ## 450    Man                          Manager
    ## 451  Woman                    Data Engineer
    ## 452  Woman                Software Engineer
    ## 453    Man              Teacher / professor
    ## 454    Man                     Data Analyst
    ## 455    Man Machine Learning/ MLops Engineer
    ## 456    Man                          Manager
    ## 457    Man                    Data Engineer
    ## 458  Woman              Teacher / professor
    ## 459  Woman Machine Learning/ MLops Engineer
    ## 460    Man                   Data Scientist
    ## 461  Woman                          Manager
    ## 462    Man                   Data Scientist
    ## 463  Woman               Research Scientist
    ## 464    Man          Engineer (non-software)
    ## 465    Man Machine Learning/ MLops Engineer
    ## 466    Man              Teacher / professor
    ## 467    Man                    Data Engineer
    ## 468    Man                          Manager
    ## 469    Man               Research Scientist
    ## 470    Man                     Data Analyst
    ## 471    Man              Teacher / professor
    ## 472    Man                   Data Scientist
    ## 473    Man Machine Learning/ MLops Engineer
    ## 474    Man                Software Engineer
    ## 475    Man Machine Learning/ MLops Engineer
    ## 476    Man              Teacher / professor
    ## 477    Man                     Data Analyst
    ## 478    Man              Teacher / professor
    ## 479    Man                     Data Analyst
    ## 480  Woman               Research Scientist
    ## 481  Woman                   Data Scientist
    ## 482    Man                   Data Scientist
    ## 483    Man              Teacher / professor
    ## 484    Man                     Data Analyst
    ## 485    Man                Software Engineer
    ## 486    Man                     Data Analyst
    ## 487    Man                Software Engineer
    ## 488    Man                   Data Scientist
    ## 489  Woman              Teacher / professor
    ## 490    Man               Research Scientist
    ## 491  Woman                     Data Analyst
    ## 492    Man                     Data Analyst
    ## 493    Man              Teacher / professor
    ## 494    Man                    Data Engineer
    ## 495    Man                     Data Analyst
    ## 496    Man                     Data Analyst
    ## 497    Man                Software Engineer
    ## 498  Woman                     Data Analyst
    ## 499    Man               Research Scientist
    ## 500    Man              Teacher / professor
    ##  [ reached 'max' / getOption("max.print") -- omitted 7810 rows ]

    gender_paygap

    ##     gender MINsal MAXsal  salary_range
    ## 1      Man  25000  29999   25000-29999
    ## 2      Man 100000 124999 100000-124999
    ## 3      Man 100000 124999 100000-124999
    ## 4      Man 200000 249999 200000-249999
    ## 5      Man 200000 249999 200000-249999
    ## 6      Man 150000 199999 150000-199999
    ## 7      Man  90000  99999   90000-99999
    ## 8      Man  30000  39999   30000-39999
    ## 9      Man  30000  39999   30000-39999
    ## 10     Man  30000  39999   30000-39999
    ## 11   Woman   3000   3999     3000-3999
    ## 12     Man  50000  59999   50000-59999
    ## 13     Man 125000 149999 125000-149999
    ## 14     Man  15000  19999   15000-19999
    ## 15     Man  50000  59999   50000-59999
    ## 16   Woman   5000   7499     5000-7499
    ## 17     Man  10000  14999   10000-14999
    ## 18     Man  30000  39999   30000-39999
    ## 19     Man  20000  24999   20000-24999
    ## 20     Man   3000   3999     3000-3999
    ## 21     Man  25000  29999   25000-29999
    ## 22     Man      0    999         0-999
    ## 23     Man  50000  59999   50000-59999
    ## 24     Man   3000   3999     3000-3999
    ## 25     Man   7500   9999     7500-9999
    ## 26     Man   4000   4999     4000-4999
    ## 27     Man 100000 124999 100000-124999
    ## 28   Woman  25000  29999   25000-29999
    ## 29     Man      0    999         0-999
    ## 30     Man  80000  89999   80000-89999
    ## 31     Man  20000  24999   20000-24999
    ## 32     Man  15000  19999   15000-19999
    ## 33     Man   3000   3999     3000-3999
    ## 34     Man      0    999         0-999
    ## 35   Woman  90000  99999   90000-99999
    ## 36     Man      0    999         0-999
    ## 37     Man   4000   4999     4000-4999
    ## 38     Man   3000   3999     3000-3999
    ## 39     Man 125000 149999 125000-149999
    ## 40     Man  50000  59999   50000-59999
    ## 41     Man  30000  39999   30000-39999
    ## 42     Man  30000  39999   30000-39999
    ## 43     Man 150000 199999 150000-199999
    ## 44     Man   3000   3999     3000-3999
    ## 45     Man   7500   9999     7500-9999
    ## 46   Woman  50000  59999   50000-59999
    ## 47   Woman      0    999         0-999
    ## 48   Woman      0    999         0-999
    ## 49     Man  20000  24999   20000-24999
    ## 50     Man   4000   4999     4000-4999
    ## 51     Man      0    999         0-999
    ## 52     Man 100000 124999 100000-124999
    ## 53     Man   3000   3999     3000-3999
    ## 54     Man   2000   2999     2000-2999
    ## 55   Woman      0    999         0-999
    ## 56     Man      0    999         0-999
    ## 57     Man  80000  89999   80000-89999
    ## 58     Man  30000  39999   30000-39999
    ## 59     Man 250000 299999 250000-299999
    ## 60     Man  20000  24999   20000-24999
    ## 61     Man      0    999         0-999
    ## 62     Man   3000   3999     3000-3999
    ## 63     Man  80000  89999   80000-89999
    ## 64     Man  90000  99999   90000-99999
    ## 65     Man 150000 199999 150000-199999
    ## 66     Man  30000  39999   30000-39999
    ## 67     Man   4000   4999     4000-4999
    ## 68     Man 125000 149999 125000-149999
    ## 69   Woman   1000   1999     1000-1999
    ## 70     Man   3000   3999     3000-3999
    ## 71     Man 500000 999999 500000-999999
    ## 72     Man 150000 199999 150000-199999
    ## 73   Woman   7500   9999     7500-9999
    ## 74     Man  70000  79999   70000-79999
    ## 75     Man   3000   3999     3000-3999
    ## 76   Woman   7500   9999     7500-9999
    ## 77   Woman      0    999         0-999
    ## 78     Man  10000  14999   10000-14999
    ## 79   Woman  10000  14999   10000-14999
    ## 80     Man      0    999         0-999
    ## 81     Man   7500   9999     7500-9999
    ## 82     Man  30000  39999   30000-39999
    ## 83     Man  10000  14999   10000-14999
    ## 84     Man  15000  19999   15000-19999
    ## 85     Man 100000 124999 100000-124999
    ## 86   Woman      0    999         0-999
    ## 87     Man   5000   7499     5000-7499
    ## 88     Man  50000  59999   50000-59999
    ## 89     Man 100000 124999 100000-124999
    ## 90     Man  10000  14999   10000-14999
    ## 91     Man   7500   9999     7500-9999
    ## 92   Woman   1000   1999     1000-1999
    ## 93     Man  50000  59999   50000-59999
    ## 94     Man      0    999         0-999
    ## 95     Man  30000  39999   30000-39999
    ## 96     Man  20000  24999   20000-24999
    ## 97     Man  60000  69999   60000-69999
    ## 98     Man  40000  49999   40000-49999
    ## 99     Man   1000   1999     1000-1999
    ## 100    Man   5000   7499     5000-7499
    ## 101  Woman      0    999         0-999
    ## 102  Woman      0    999         0-999
    ## 103  Woman 100000 124999 100000-124999
    ## 104    Man      0    999         0-999
    ## 105    Man  50000  59999   50000-59999
    ## 106    Man 125000 149999 125000-149999
    ## 107    Man  50000  59999   50000-59999
    ## 108    Man   3000   3999     3000-3999
    ## 109    Man 150000 199999 150000-199999
    ## 110    Man 150000 199999 150000-199999
    ## 111    Man  30000  39999   30000-39999
    ## 112    Man  90000  99999   90000-99999
    ## 113    Man  20000  24999   20000-24999
    ## 114    Man  40000  49999   40000-49999
    ## 115    Man   5000   7499     5000-7499
    ## 116    Man  30000  39999   30000-39999
    ## 117    Man 100000 124999 100000-124999
    ## 118    Man  25000  29999   25000-29999
    ## 119    Man 125000 149999 125000-149999
    ## 120    Man   5000   7499     5000-7499
    ## 121    Man  10000  14999   10000-14999
    ## 122    Man   1000   1999     1000-1999
    ## 123  Woman  15000  19999   15000-19999
    ## 124    Man   5000   7499     5000-7499
    ## 125  Woman  15000  19999   15000-19999
    ## 126    Man  15000  19999   15000-19999
    ## 127    Man  15000  19999   15000-19999
    ## 128  Woman   2000   2999     2000-2999
    ## 129  Woman  60000  69999   60000-69999
    ## 130    Man  90000  99999   90000-99999
    ## 131    Man  30000  39999   30000-39999
    ## 132    Man   1000   1999     1000-1999
    ## 133    Man   7500   9999     7500-9999
    ## 134    Man   4000   4999     4000-4999
    ## 135    Man      0    999         0-999
    ## 136    Man  40000  49999   40000-49999
    ## 137    Man  20000  24999   20000-24999
    ## 138  Woman  40000  49999   40000-49999
    ## 139    Man  60000  69999   60000-69999
    ## 140  Woman   5000   7499     5000-7499
    ## 141    Man  25000  29999   25000-29999
    ## 142  Woman      0    999         0-999
    ## 143  Woman  80000  89999   80000-89999
    ## 144  Woman  50000  59999   50000-59999
    ## 145    Man  40000  49999   40000-49999
    ## 146    Man      0    999         0-999
    ## 147    Man   4000   4999     4000-4999
    ## 148    Man   2000   2999     2000-2999
    ## 149    Man 125000 149999 125000-149999
    ## 150    Man   2000   2999     2000-2999
    ## 151    Man      0    999         0-999
    ## 152  Woman   4000   4999     4000-4999
    ## 153    Man  40000  49999   40000-49999
    ## 154    Man   7500   9999     7500-9999
    ## 155    Man   1000   1999     1000-1999
    ## 156    Man   7500   9999     7500-9999
    ## 157    Man  80000  89999   80000-89999
    ## 158    Man   5000   7499     5000-7499
    ## 159    Man  25000  29999   25000-29999
    ## 160    Man  30000  39999   30000-39999
    ## 161    Man  25000  29999   25000-29999
    ## 162    Man   1000   1999     1000-1999
    ## 163    Man  15000  19999   15000-19999
    ## 164    Man  70000  79999   70000-79999
    ## 165    Man  70000  79999   70000-79999
    ## 166  Woman  25000  29999   25000-29999
    ## 167    Man  25000  29999   25000-29999
    ## 168    Man      0    999         0-999
    ## 169    Man      0    999         0-999
    ## 170    Man  20000  24999   20000-24999
    ## 171    Man 200000 249999 200000-249999
    ## 172    Man 125000 149999 125000-149999
    ## 173    Man   3000   3999     3000-3999
    ## 174    Man  20000  24999   20000-24999
    ## 175  Woman  40000  49999   40000-49999
    ## 176    Man  20000  24999   20000-24999
    ## 177    Man 125000 149999 125000-149999
    ## 178    Man  80000  89999   80000-89999
    ## 179    Man   5000   7499     5000-7499
    ## 180    Man 125000 149999 125000-149999
    ## 181  Woman      0    999         0-999
    ## 182    Man 125000 149999 125000-149999
    ## 183    Man 100000 124999 100000-124999
    ## 184    Man  50000  59999   50000-59999
    ## 185  Woman 100000 124999 100000-124999
    ## 186  Woman      0    999         0-999
    ## 187    Man   2000   2999     2000-2999
    ## 188    Man 250000 299999 250000-299999
    ## 189    Man      0    999         0-999
    ## 190    Man  40000  49999   40000-49999
    ## 191    Man   1000   1999     1000-1999
    ## 192    Man  50000  59999   50000-59999
    ## 193    Man   3000   3999     3000-3999
    ## 194    Man      0    999         0-999
    ## 195    Man      0    999         0-999
    ## 196    Man   4000   4999     4000-4999
    ## 197    Man      0    999         0-999
    ## 198    Man  90000  99999   90000-99999
    ## 199    Man  40000  49999   40000-49999
    ## 200    Man      0    999         0-999
    ## 201  Woman 100000 124999 100000-124999
    ## 202    Man  40000  49999   40000-49999
    ## 203    Man  40000  49999   40000-49999
    ## 204  Woman  70000  79999   70000-79999
    ## 205    Man  70000  79999   70000-79999
    ## 206    Man  70000  79999   70000-79999
    ## 207    Man  40000  49999   40000-49999
    ## 208    Man 100000 124999 100000-124999
    ## 209    Man  10000  14999   10000-14999
    ## 210  Woman   3000   3999     3000-3999
    ## 211    Man   1000   1999     1000-1999
    ## 212    Man   3000   3999     3000-3999
    ## 213    Man  30000  39999   30000-39999
    ## 214    Man  15000  19999   15000-19999
    ## 215    Man  80000  89999   80000-89999
    ## 216  Woman  50000  59999   50000-59999
    ## 217    Man 200000 249999 200000-249999
    ## 218    Man   2000   2999     2000-2999
    ## 219  Woman      0    999         0-999
    ## 220    Man   1000   1999     1000-1999
    ## 221  Woman  80000  89999   80000-89999
    ## 222  Woman   7500   9999     7500-9999
    ## 223    Man   7500   9999     7500-9999
    ## 224  Woman 150000 199999 150000-199999
    ## 225    Man   3000   3999     3000-3999
    ## 226    Man  50000  59999   50000-59999
    ## 227    Man   3000   3999     3000-3999
    ## 228    Man  25000  29999   25000-29999
    ## 229    Man  10000  14999   10000-14999
    ## 230    Man  25000  29999   25000-29999
    ## 231    Man  70000  79999   70000-79999
    ## 232    Man   3000   3999     3000-3999
    ## 233    Man   5000   7499     5000-7499
    ## 234  Woman   1000   1999     1000-1999
    ## 235    Man  70000  79999   70000-79999
    ## 236    Man  30000  39999   30000-39999
    ## 237    Man 125000 149999 125000-149999
    ## 238    Man  90000  99999   90000-99999
    ## 239    Man   5000   7499     5000-7499
    ## 240    Man  10000  14999   10000-14999
    ## 241    Man  50000  59999   50000-59999
    ## 242  Woman   1000   1999     1000-1999
    ## 243    Man 150000 199999 150000-199999
    ## 244    Man      0    999         0-999
    ## 245    Man      0    999         0-999
    ## 246    Man 200000 249999 200000-249999
    ## 247    Man      0    999         0-999
    ## 248    Man   7500   9999     7500-9999
    ## 249    Man   2000   2999     2000-2999
    ## 250    Man  80000  89999   80000-89999
    ##  [ reached 'max' / getOption("max.print") -- omitted 7745 rows ]

    salrange_density <- df1 %>% select("Q3","Q4", "Q23", "Q29")

    salrange_density <- salrange_density %>% 
      #filter(Q4 == "United States of America") %>% #filter to US salaries only
      filter(Q3 == "Man" | Q3 == "Woman") %>% #confine to man/woman
      filter(Q23 != "Currently not employed" & Q23 != "Other") %>% #remove unemployed/other
      filter(Q29 != ">$1,000,000") %>% #remove millionaires 
      mutate_all(na_if, "") #fill blank data with NA

    salrange_density <- salrange_density %>% filter_at(vars(Q23, Q29), all_vars(!is.na(.))) 
    #remove incomplete rows (left with ~7,000 rows)


    salrange_density <- salrange_density %>% mutate(Q23 = replace(Q23, Q23 == "Data Analyst (Business, Marketing, Financial, Quantitative, etc)", "Data Analyst")) %>% mutate(Q23 = replace(Q23, Q23 == "Manager (Program, Project, Operations, Executive-level, etc)", "Manager")) 
    #simplifying strings of some job roles

    #split salary range column(Q29), turn to numeric dtype, then add factor type column
    salrange_density <- salrange_density  %>% mutate(across(starts_with("Q29"), ~gsub("\\$", "", .))) %>% mutate(across(starts_with("Q29"), ~gsub("\\,", "", .)))
    #strip out $ and commas

    salrange_density <- salrange_density %>% separate(Q29, c("MINsal", "MAXsal"))
    #string split

    salrange_density$MINsal <- as.numeric(salrange_density$MINsal)
    salrange_density$MAXsal <- as.numeric(salrange_density$MAXsal)
    #as numeric conversion

    salrange_density <- salrange_density %>% 
      mutate(
        salary_range = paste0 (
      format(MINsal, trim = TRUE),
      "-", 
      MAXsal
    ),
    salary_range = fct_reorder(salary_range, MINsal)
      )
    #re-combine newly separated column and create a corresponding factor column

    salrange_density <- salrange_density %>% rename("gender" = "Q3", "job_title" = "Q23")

    head(salrange_density)

    ##   gender                       Q4          job_title MINsal MAXsal  salary_range
    ## 1    Man                   France     Data Scientist  25000  29999   25000-29999
    ## 2    Man                  Germany  Software Engineer 100000 124999 100000-124999
    ## 3    Man                Australia Research Scientist 100000 124999 100000-124999
    ## 4    Man United States of America Developer Advocate 200000 249999 200000-249999
    ## 5    Man United States of America     Data Scientist 200000 249999 200000-249999
    ## 6    Man United States of America     Data Scientist 150000 199999 150000-199999

    #col_grid <- rgb(235, 235, 235, 100, maxColorValue = 300)

visualization attempt

    ggplot(salrange_density, aes(x=job_title, y=salary_range)) +
      
      geom_jitter(aes(colour=gender), height = 0.3, width = 0.3, alpha=0.8)+
      
      scale_color_manual(values=c("deepskyblue","deeppink")) +
      
       labs(x="job title", y="salary range, annual (USD)") +
      
      theme_minimal() +
      
      theme(axis.text.x = element_text(angle = 45, hjust=1),
            axis.text = element_text(size=rel(2)),
            axis.title = element_text(size=rel(1)))

![](finalproj_files/figure-markdown_strict/unnamed-chunk-76-1.png)

    #ggsave("whiteUSsalary_density.png", width = 7000, height = 4096, units="px")

#### scatter brain

##### Part 1. gender ratiod donut chart

    df1 %>% select(Q2) %>% gather("key", "value") %>% group_by(value) %>% summarise(n=n()) %>% arrange((value))

    ## # A tibble: 11 × 2
    ##    value     n
    ##    <chr> <int>
    ##  1 18-21  4559
    ##  2 22-24  4283
    ##  3 25-29  4472
    ##  4 30-34  2972
    ##  5 35-39  2353
    ##  6 40-44  1927
    ##  7 45-49  1253
    ##  8 50-54   914
    ##  9 55-59   611
    ## 10 60-69   526
    ## 11 70+     127

    age_demo <- df1 %>% select(Q2) %>% gather("key", "value") %>% group_by(value) %>% summarise(n=n()) %>% arrange((value))

    ggplot(age_demo, aes(x=n, y=value)) +
      geom_col(fill="gold") +
      scale_x_continuous(expand = c(0, 0)) + 
      scale_x_reverse() +
      scale_y_discrete(position="right") +
      geom_vline(xintercept = 0, linetype="dashed") +
      theme_minimal() +
      labs(x="count", y="") +
      theme(panel.grid.major = element_blank(),
            panel.grid.minor = element_blank())

    ## Scale for x is already present.
    ## Adding another scale for x, which will replace the existing scale.

![](finalproj_files/figure-markdown_strict/unnamed-chunk-79-1.png)

    #ggsave("age_demographics.png")

    age_demo

    ## # A tibble: 11 × 2
    ##    value     n
    ##    <chr> <int>
    ##  1 18-21  4559
    ##  2 22-24  4283
    ##  3 25-29  4472
    ##  4 30-34  2972
    ##  5 35-39  2353
    ##  6 40-44  1927
    ##  7 45-49  1253
    ##  8 50-54   914
    ##  9 55-59   611
    ## 10 60-69   526
    ## 11 70+     127

##### Part 2. programming donutchart

    df1 %>% select(contains("Q12_")) %>% gather("key", "value") %>% mutate_all(na_if, "") %>% drop_na() %>% group_by(value) %>% summarise(n=n()) %>% arrange(desc(n))

    ## # A tibble: 15 × 2
    ##    value          n
    ##    <chr>      <int>
    ##  1 Python     18653
    ##  2 SQL         9620
    ##  3 R           4571
    ##  4 C++         4549
    ##  5 Java        3862
    ##  6 C           3801
    ##  7 Javascript  3489
    ##  8 MATLAB      2441
    ##  9 Bash        1674
    ## 10 C#          1473
    ## 11 PHP         1443
    ## 12 Other       1342
    ## 13 Go           322
    ## 14 Julia        296
    ## 15 None         256

    pop_language <- df1 %>% select(contains("Q12_")) %>% gather("key", "value") %>% mutate_all(na_if, "") %>% drop_na() %>% group_by(value) %>% summarise(n=n()) %>% arrange(desc(n))

reformat keys to simplify our donut chart

    pop_language <- pop_language %>% mutate(value = ifelse(n < 3000, "Other*", value)) %>% group_by(value) %>% summarise(n = sum(n)) %>% arrange(desc(n))

    pop_language

    ## # A tibble: 8 × 2
    ##   value          n
    ##   <chr>      <int>
    ## 1 Python     18653
    ## 2 SQL         9620
    ## 3 Other*      9247
    ## 4 R           4571
    ## 5 C++         4549
    ## 6 Java        3862
    ## 7 C           3801
    ## 8 Javascript  3489

    pop_language$fraction <- pop_language$n / sum(pop_language$n)

    pop_language$ymax <- cumsum(pop_language$fraction)

    pop_language$ymin <- c(0, head(pop_language$ymax, n=-1))

    # Compute label position
    pop_language$labelPosition <- (pop_language$ymax + pop_language$ymin) / 2

    pop_language$label <- paste0(pop_language$category, "\n value: ", pop_language$count)

    ## Warning: Unknown or uninitialised column: `category`.

    ## Warning: Unknown or uninitialised column: `count`.

making the plot

    ggplot(pop_language, aes(ymax=ymax, ymin=ymin, xmax=4, xmin=3, fill=value)) +
      geom_rect() +
      geom_label(x=3.5, aes(y=labelPosition, label=value)) +
      #geom_label( x=3.5, aes(y=labelPosition, label=label), size=6) +
      scale_fill_brewer(palette= "Set2") +
      coord_polar(theta="y") +
      xlim(c(0, 4)) +
      theme_void() +
      theme(legend.position = "none") +
      labs(caption="*Other includes: C#, MATLAB, Bash, PHP, Go, and Julia")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-86-1.png)

    #ggsave("donutchart_lang.png")

    pop_language

    ## # A tibble: 8 × 7
    ##   value          n fraction  ymax  ymin labelPosition label       
    ##   <chr>      <int>    <dbl> <dbl> <dbl>         <dbl> <chr>       
    ## 1 Python     18653   0.323  0.323 0             0.161 "\n value: "
    ## 2 SQL         9620   0.166  0.489 0.323         0.406 "\n value: "
    ## 3 Other*      9247   0.160  0.649 0.489         0.569 "\n value: "
    ## 4 R           4571   0.0791 0.728 0.649         0.689 "\n value: "
    ## 5 C++         4549   0.0787 0.807 0.728         0.768 "\n value: "
    ## 6 Java        3862   0.0668 0.874 0.807         0.840 "\n value: "
    ## 7 C           3801   0.0658 0.940 0.874         0.907 "\n value: "
    ## 8 Javascript  3489   0.0604 1     0.940         0.970 "\n value: "
