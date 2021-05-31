# Complementary Performance

This repository contains task examples and the collected experiment data for [our CHI 2021 paper](https://arxiv.org/pdf/2006.14779.pdf). If you are interested in this work, please consider citing our paper:

```bibtex
@inproceedings{bansal2021complementary,
    author = {Bansal, Gagan and Wu, Tongshuang and Zhou, Joyce and Fok, Raymond and Nushi, Besmira and Kamar, Ece and Ribeiro, Marco Tulio and Weld, Daniel},
    title = {Does the Whole Exceed Its Parts? The Effect of AI Explanations on Complementary Team Performance},
    year = {2021},
    isbn = {9781450380966},
    publisher = {Association for Computing Machinery},
    address = {New York, NY, USA},
    url = {https://doi.org/10.1145/3411764.3445717},
    doi = {10.1145/3411764.3445717},
    booktitle = {Proceedings of the 2021 CHI Conference on Human Factors in Computing Systems},
    articleno = {81},
    numpages = {16},
    keywords = {Human-AI teams, Augmented intelligence, Explainable AI},
    location = {Yokohama, Japan},
    series = {CHI '21}
}
```

## Data formats

### Task examples

Below, we explain the json format of the task examples. For their creation details (including the source datasets, annotation procedure, and prediction models), please refer to our paper.

For _AmzBook_ and _Beer_ examples:
```js
{
    "testid": /* unique identifier */,
    "X": /* The input data point */,
    "Y": /* groundtruth label */,
    "pred": /* the label from our RoBERTa model used for the experiment */,
    "conf": /* model prediction accuracy */,
    "expert": /* Explanation, annotated by experts. The text is processed into HTML format, with <span class='class0|class1'> denoting explanation for the positive/negative class. */,
    "system": /* Highlight-based explanation, annotated by LIME. */
    }
```

For _LSAT_ examples:

```js
{
    "id": "unique identifier",
    "choices": { /* the four options in LSAT as keys, A-D */
      "A": {
        "text": /* the text of the option */,
        "explanation": /* the explanation text, either from the textbook or annotated by the expert */
      }
    },
    "question": /* Context of the LSAT instance */,
    "prompt": /* The test prompt */,
    "answer": /* the groundtruth label */,
    "jarvisAnswer1": /* Top choice of the model */,
    "jarvisAnswer2": /* Second top choice of the model */,
    "jarvisConf1": /* confidence score for the top choice */,
    "jarvisConf2": /* confidence score for the second top choice */,
}
```

### Experiment data

The experiment data include the human-AI team decisions per question, per crowdworker and the survey texts, in the csv format. We have anonymized the results, such that each worker is represented by a randomly generated id.

Our analyses are on `-filter` data points:
> removed data from participants whose median labeling time was less than 2 seconds or
those who assigned the same label to all examples. (sec 4.4)

but we've also opened the full, `-prefilter` data points.

For _Team Decisions_:

```yml
assignmentId: # the randomly generated, anonymized id that identifies a crowdworker.
questionId: # the order of the questions per task.
task: # the experiment task,
condition: # the experiment condition,
time: # the completion time for the one question, 
choice: # the final decision of the human worker,
y: # the groundtruth label,
pred: # the top-1 prediction from the model,
conf: # the corresponding confidence of the top 1 prediction,
conf2: # the top-2 prediction from the model. Only available for LSAT; For the binary Beer and AmzBook, it is just 1-conf.
pred2: # the corresponding confidence of the top 2 prediction, only available for LSAT.
```

For _Survey results_:

```yml
assignmentId: # the randomly generated, anonymized id that identifies a crowdworker.
question: # a readable alias denoting the survey question.
    - assist_is_useful: "Marvin's assistance (e.g. the information it displayed) helped me solve the task." [likert scale],
    - expl_is_useful: "Marvin’s explanations in particular helped me solve the task." [likert scale],
    - assit_useful_detail: "In two sentences, describe how you used the information that Marvin provided." [freeform text],
    # two additional entries are "Did you experience any technical issues while doing this task", and "Do you have any feedback to improve the HIT?"
condition: # the experiment condition,
task: # the experiment task,
```
