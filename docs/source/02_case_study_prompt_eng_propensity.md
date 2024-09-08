# 02. Causal interpretability for Software Engineering 

This notebook is for causal effect analysis of different treatments on code completion quality of LLMs in software engineering. Using DoWhy library in Python we analyze the impacr of different code completion prompts on the Levenstein distance between the generated code and the ground truth (GT).

Our goal is to analyze how different code completion promprs might affect the similarity between the generated and original code. The reults of this analysis could help improve the LLMs for code completion and understand the impact of prompt emgineering in SWE tasks.


## Step 1: Get the data
We first load the Galeras datasets for prompt engineering by SEMERU lab. This comprehensive dataset contains:
1. The data with control prompts;
2. Treatment 1 (T1) prompts;
3. Treatment 2 (T2) prompts;
4. **Treatment: Levenstein**  ...


## Step 2: Data Pre-processing and analysis
1. Normalize the column y_po_lev (Levenstein distance) for code predictions;
2. Visualize both the original and normalized Levenstein variables against the various treatments to study the differences in distributions and identify the outliers.

## Step 3: Causal Effect Inference
1. Causal model creation: 
Here we create three causal models (m1, m2, m3) with increasing complexity for each treatment T1 and T2 (total of 6 models). We set a list of confounder (w) variables as |common_causes| and construct a graph. The graph shows all the variables from the dataset represented as nodes and all the causal relationships represented as edges. The arrows on the edges indicate the direction of causality (pointing from the causes to the effects).

Our variables include (labeled using letters <i, e, w, y> in front of the variable names):
    - Treatment (i) variables that we have manipulated and are the _causes_ in our cause-and-effect relationships.
    - Outcome (e) variables that we measure to identify how they were impcated by the treatment(s) and are the _effects_ in our cause-and-effect relationships.
    - Confounding (w) variables that impact both the treatement and the outcome , wchih create false associations. 
    - Levenstein distance (y) 

First model (m1) estimates the effects of the confounding variables on the binary treatment variable and the normalized Levenstein's distance. Then the second model (m2) estimates the effect of the treatment variable _i_vocab_size_  in addition to the confounding variables. Lastly, the third model (m3) estimates the effect of the outcome variable _e_n_words_ in addition to the variables from *m2*. e run these models twice for each Treatment set 1 and 2. 

2. Cuasal Effect Estimation: Here we produce estimands to identify the causal effect using instrumental variable method. This method is utlized when unobserved confounding variables might introduce bias to the estimated effect/outcome. 
In addition, we utilize the Propensity Score Matching, Propensity Score Stratification, and Propensity Score Weighting to estimate the causal effects. 

3. Validation: We perform a Placebo Treatment and Random Common Cause refutation to validate our results. 

    Analyzing the outputs of the causal model.
    - The instrumental variable method estimates a mean effect of 1071.29 with p-value < 0.001. 
    - The propensity score mathcing method produces mean effect of 9.218
    - The placebo treatment refutation shows a new effect of 1041.10 compaed to the estimated effect of 1071.29 (p-value = 0).
    - Correlation analysis between confounding variables shows strong correlation between some variables. For example, w_token_counts and w_n_ast_nodes are strong correlation score of 0.92 for both Pearson and Spearman methods. 


## Key findings:
1. Through causal effect estimation we observe that the treatments (varying prompting strategies) have a ... effect on the normalized Levinstein distance between the generated code and GT. 

2. Are we seeing positive/negative effects of the treatment on the outcome?

3. Propensity score vvs instumental variable method?

4. Do validation tests support the robustness of our findings?







**Sources**:

https://causalwizard.app/inference/article/iv

https://causalwizard.app/inference/article/random-common-cause#:~:text=By%20adding%20a%20random%20common%20cause%2C%20we%20introduce%20a%20new,the%20cause%20and%20the%20effect.
































