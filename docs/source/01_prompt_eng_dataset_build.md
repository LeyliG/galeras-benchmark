# 01. Causal interpretability for Software Engineering 

This notebook is for causal effect analysis for software engineering. To perform the analysis we build the dataset of code samples and various prompting strategies. Using DoWhy library in Python we analyze the impacr of different code completion prompts on the Levenstein distance between the generated code and the ground truth (GT).

Our goal is to analyze how different code completion promprs might affect the similarity between the generated and original code. The reults of this analysis could help improve the LLMs for code completion and understand the impact of prompt emgineering in SWE tasks.


## Step 1: Get the data
We first load the Galeras datasets by SEMERU lab which are hosted on Hugging Face:
1. Control dataset containing the data with control prompts;
2. Treatment 1  (T1) dataset with the T1 prompts;
3. Treatment 2 (T2) dataset with the T2 prompts;
4. **Treatment: Levenstein** dataset containing ...


## Step 2: Data Pre-processing
Next, we perform data pre-processing:
1. Flatten the nested data structures;
2. Rename columns for consistency;
3. Combine the datasets into a single dataframe (i.e. joining on commit_id)
4. Calculate the Levenstein distance between the original code and the generated code

## Step 3: Causal Effect Inference
1. Causal model creation: Here we create a causal model using T1 and control promtps. We set a list of confounder (w) variables as |common_causes| and construct a graph. The graph shows all the variables from the dataset represented as nodes and all the causal relationships represented as edges. The arrows on the edges indicate the direction of causality (pointing from the causes to the effects).

Our variables include (labeled using letters <i, e, w, y> in front of the variable names):
    - Treatment (i) variables that we have manipulated and are the _causes_ in our cause-and-effect relationships.

    - Outcome (e) variables that we measure to identify how they were impcated by the treatment(s) and are the _effects_ in our cause-and-effect relationships.

    - Confounding (w) variables that impact both the treatement and the outcome , wchih create false associations. 

    - Levenstein distance (y) 

2. Cuasal Effect Estimation: Here we produce estimands to identify the causal effect using instrumental variable method. This method is utlized when unobserved confounding variables might introduce bias to the estimated effect/outcome. 
We also perform a placebo treatment refutation to validate our results. 
Finally, we calculate and analyze the correlations between confounding variables. 


    Analyzing the outputs of the causal model.
    - The instrumental variable method estimates a mean effect of 1071.29 with p-value < 0.001. 

    - The propensity score mathcing method produces mean effect of 9.218

    - The placebo treatment refutation shows a new effect of 1041.10 compaed to the estimated effect of 1071.29 (p-value = 0).
    
    - Correlation analysis between confounding variables shows strong correlation between some variables. For example, w_token_counts and w_n_ast_nodes are strong correlation score of 0.92 for both Pearson and Spearman methods. 


## Key findings:
1. Through causal effect estimation we observe that the treatments (varying prompting strategies) have a significant effect on the Levinstein distance between the generated code and GT. 

2. We observe strong correlations between various confounding variables (common causes) which coulkd influence the causal effect estimation. 

3. Propensity score vvs instumental variable method?







**Sources**:
https://causalwizard.app/inference/article/iv










