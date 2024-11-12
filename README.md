# CIFAR_MIA
Membership Inference Attacks on CIFAR 10 dataset.

This is a new approach to Membership Inference attacks using the shadow model approach as presented in the paper[Membership Inference Attacks Against Machine Learning Models](https://ieeexplore.ieee.org/document/7958568) by Shokri et al.

## What did I do?
1. I followed the architecture mentioned in the paper and trained the model on the orginal [CIFAR dataset](https://www.cs.toronto.edu/~kriz/cifar.html) and evaluated it on the orginal test dataset of CIFAR 10.
2. I then used the [CIFAKE](https://www.kaggle.com/datasets/birdy654/cifake-real-and-ai-generated-synthetic-images) dataset to train 5 shadow models that have the same architecture of the target model.
3. The train and test metrics of the target models and 5 shadow models are presented below.

| Model  | Train Accuracy | Train Precision | Train Recall | Train F1 Score | Test Accuracy | Test Precision | Test Recall | Test F1 Score |
|--------|----------------|-----------------|--------------|----------------|---------------|----------------|-------------|---------------|
| Baseline | 95.47%        | 0.9548         | 0.9546      | 0.9547        | 60.56%        | 0.6016         | 0.6056      | 0.6031        |
| SM1     | 94.19%        | 0.9422         | 0.9417      | 0.9419        | 58.92%        | 0.5950         | 0.5892      | 0.5908        |
| SM2     | 94.81%        | 0.9480         | 0.9483      | 0.9481        | 59.34%        | 0.5923         | 0.5934      | 0.5924        |
| SM3     | 95.41%        | 0.9542         | 0.9540      | 0.9540        | 59.65%        | 0.5934         | 0.5965      | 0.5944        |
| SM4     | 95.23%        | 0.9522         | 0.9521      | 0.9521        | 60.15%        | 0.5972         | 0.6015      | 0.5986        |


4. I then generated attack data for the Attack model using the following method. I took the test subsection of the CIFAKE dataset divided into 5 equal parts creating 5 dataloader.
5. For each shadow model I then obtained the last layer softmax predictions or "logits" and stored them as the feature vectors for the attack model and assigned a label of 1 for the logits for the examples from the training dataset of the shadow models and a label 0 for the logits for the examples from one of the splits of the testdataset of the shadow model.
6. Now the attack model is then trained on this generated dataset and using pycaret I found out that a 	Gradient Boosting Classifier performed well.
7. I then evaluated the efficacy of the MIA and the metrics are presented below.

Precision: 0.8669693476675527
Recall: 0.73708
F1 Score: 0.7967657200920991
Accuracy: 0.68665"


## Threat model

We assume that the attacker has access to the follwing parameters
1. The outputs of the final layer of the target model.
2. The architecture of the target model.
3. The attacker generates a synthetic dataset (CIFAKE) and uses this dataset to train shadow models.

