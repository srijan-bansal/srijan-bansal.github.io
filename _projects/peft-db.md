---
layout: page
title: Parameter Efficient Debiasing of LMs
description: Using PEFTs for generic model debiasing
img: assets/img/peftdb.png
importance: 1
category: PEFT
github: https://github.com/sumit-agrwl/peft-debias
---

In the realm of artificial intelligence, the presence of biases within trained models has emerged as a pressing concern. These biases, often ingrained in the datasets used for training, can lead to skewed outcomes due to correlations between protected attributes and corresponding labels. To illustrate this issue, let's delve into a specific example involving a BERT model.

Imagine a BERT model trained on a dataset that contains biases. Consider a scenario where a biography states, "His portfolio includes the most luxurious, beautiful, expensive, and large-scale projects in the world. For example, he made interior design." Despite the accurate label being "Interior Designer," the model might mistakenly classify this individual as an "Architect."

So how does such a misclassification occur? The answer lies in the model's absorption of biases present in the training data. In this case, the model has learned to associate Interior Design predominantly with females, while reserving the label of Architect for males. Consequently, it wrongly assigns the male individual mentioned in the biography to the wrong profession.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/peftdb_tpr.png" title="TPR for BERT" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Figure 1 demonstrates the disparity in true positive rates between male and female predictions for various occupations. The model exhibits biases towards males for professions like surgeon, engineer, and architect (indicated by the color blue), while demonstrating biases towards females for occupations like designer and nurse (highlighted in red).

To combat these biases, we present a groundbreaking approach called Parameter Efficient Debiasing (PEFTDB). This method serves as an alternative to traditional full model training for debiasing. PEFTDB consists of two phases: an upstream phase that captures debiasing information using PEFTs along a specific bias axis (e.g., gender), and a downstream phase where these debiasing parameters are frozen during model fine-tuning. We utilize counterfactual data augmentation (CDA)[1] on the source data for debiasing, which involves swapping attribute words associated with bias (e.g., he/she for gender) in a corpus. By generating counterfactual examples, we can mitigate biases in the data.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/peftdb_model.png" title="TPR for BERT" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Our research focuses on evaluating the effectiveness of PEFTDB across different PEFTs and analyzing multiple bias axes, including gender and group biases. We assess gender bias using the BiasBios dataset, commonly employed in occupation prediction tasks. For evaluating group bias associated with race, religion, and sexual orientation, we utilize datasets such as StormFront, GAB, and FDCL. Additionally, we examine the generalizability of our task-agnostic parameters captured along a specific bias axis by evaluating their efficacy on different downstream datasets exhibiting bias along the same axis.

Through extensive experimentation, we provide compelling evidence that PEFTDB is highly effective in mitigating biases in downstream tasks, particularly regarding gender and group axes. Among the various PEFT approaches we explored, Prompt Tuning and Sparse Fine Tuning consistently outperformed other techniques, highlighting their superiority in debiasing. Furthermore, we demonstrate the task-agnostic nature of our approach by leveraging group-based parameters learned from datasets like Stormfront or FDCL during the upstream phase. These parameters effectively debias models when applied to the GHC dataset, showcasing the versatility of our debiasing approach. The task-agnostic parameters acquired from one dataset can successfully address biases in a different dataset within the same group axis. You can find the full report <a href="{{ '/assets/pdf/peftdb.pdf' }}">here</a>.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/peftdb_table.png" title="TPR for BERT" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<b>References</b>

[1] R Zmigrod, S J Mielke, H Wallach, R Cotterell, <a href="https://aclanthology.org/P19-1161v2.pdf">Counterfactual Data Augmentation for Mitigating Gender Stereotypes in Languages with Rich Morphology</a>
