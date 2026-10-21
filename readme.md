# Explanation of Positional Encoding in Transformer

1. Introduction
This series of tutorials will delve into the different modules of the Transformer architecture, exploring their roles in signal classification tasks such as fault diagnosis. Specifically, we will examine which modules are active and how they contribute to these tasks.
Preface
In this issue, based on data from Case Western Reserve University (CWRU) bearings, a detailed introduction to positional encoding within the Transformer is provided, while also exploring its influence on fault classification tasks.
1. Positional Encoding Introduction
1.1 Order Information in Data
In any language, the order of words in a sentence is crucial for conveying meaning. In traditional recurrent neural networks (RNN), the model processes the input sequence word by word, with each output at a given moment depending on the previous one. This sequential processing approach makes RNN inherently suitable for handling sequence data, as it can implicitly preserve the order information of words within the sequence.
1.2 Concept of Positional Encoding
Since the Transformer model does not have an RNN or CNN structure, words enter the network simultaneously, so there is no explicit relative or absolute information about the position of each word in the source sentence. To enable the model to understand the position of each word in the sequence, the Transformer paper introduces a technique called Positional Encoding. This technique involves adding an additional encoding to each word to represent its position in the sequence, allowing the model to comprehend the relative positions of words within the sequence.
The RNN serves as a feature extractor, capturing the order information of words; however, the Attention mechanism does not consider the order information. However, the order information has a significant impact on semantics, thus it is necessary to incorporate positional information into the input embedding through Positional Embedding.
In summary, Positional Encoding involves adding (embedding) positional information to the word vectors, allowing the Transformer to preserve the positional information of the word vectors, thereby enhancing the model's ability to understand sequences.
1.3 Classification of Positional Encoding:
There are two mainstream methods for positional encoding in transformer models:
(1) Absolute Positional Encoding:
The Learned Positional Embedding method is the most common absolute positional encoding method. This method directly initializes a positional embedding randomly at different positions and adds it to the word embedding before inputting it into the model for training as a parameter.
(2) Relative Positional Encoding:
Using absolute positional encoding, while the positional embeddings corresponding to different positions differ, the distance between position 1 and position 2 is closer than that between position 3 and position 10, and both position 1 and position 2, as well as position 3 and position 4, are only separated by 1. These represent relative positional encoding.
The commonly used relative position encoding methods include Sinusoidal Positional Encoding and Learned Positional Encoding. Sinusoidal Positional Encoding involves applying different frequencies of sine and cosine functions to the positions of input sequences to calculate positional encoding; whereas, Learned Positional Encoding calculates positional encoding by learning a set of learnable parameters.

(3) Complex Encoding - Complex Embedding
In "Attention is all you need", it was mentioned that the effectiveness of Learned Positional Embedding and Sinusoidal Position Encoding are not significantly different. In the paper "Encoding Word Order in Complex Embeddings", experimental results show that using Complex embeddings has a significant improvement compared to the previous two methods.

1.4 Position Vectors and Word Vectors
Generally, one can combine position vectors and word vectors by either vector concatenation or addition. This essentially performs matrix stacking on both the input data and the position information data!

Here, `input_embedding` is obtained through a standard Embedding layer, mapping the dimensionality of each token's vector from the original data dimensions to `d_model`. Since this is an addition relationship, `positional_encoding` also has a dimension of `d_model`. (In the original paper, `d_model` is set to 512.)

Regarding `d_model`:
2. Positional Encoding Principle
2.1 Principle Explanation
In the Transformer paper, absolute position is represented using a cosine function, and relative position is obtained by multiplying these two functions. Since the cosine function has periodicity, it can effectively represent the relative positions of words in a sequence. We will illustrate this with the example of Sinusoidal Positional Encoding.
First, let's interpret the formula from the paper and provide corresponding code for Positional Encoding:
The formula for Positional Encoding is as follows:
Here is the corresponding code implementation:
From the formula, it can be seen that the positional encoding of a word is composed of different frequency cosine functions. The advantage of this design is that the positional encoding at position + k can be linearly represented by position pos, reflecting its relative position relationship.
Although Sinusoidal Position Encoding seems complex, it can be proven that position + k can be linearly represented by position pos, which requires only high school trigonometric functions:
1. Positional encoding for pos+k:
It can be observed that for the position vector of a certain dimension (2i or 2i+1) at the position pos + k, it can be represented as a linear combination of the position vectors of pos and k positions. This linear combination implies that the position vector contains relative position information.

2.2 Common understanding:
The simplest and most intuitive way to add position information is by using one-hot encoding. For example, converting the sentence into one-hot representation:
The numbers formed by combining dimensions 0, 1, 2, 3 are the binary representations corresponding to each position. It can be seen that each dimension (each column) has its own cycle, and the cycles are different. Specifically, the rate of change for each bit is different; the lower the bit, the faster the change (the more towards the right side, the faster the change frequency). The red position 0 and 1 will change once each, while the yellow position changes once every 8 digits. This demonstrates that multiple cycles with different periods can be equivalent when composed of multi-dimensional encodings and increasing sequences. This also answers why period functions can introduce position information.

Finally, we need to combine the position vector with the word vector. Generally, we can combine them by either concatenating vectors or directly adding them. Let's illustrate this with a case study on bearing fault data:

3. Preprocessing of bearing fault data:
2.1 Importing data:
Refer to previous articles for preprocessing of fault classification, specifically the bearing data from Case Western Reserve University with 10 categories dataset:

train_set, val_set, test_set are divided according to a ratio of 7:2:1 for training set, validation set, and test set respectively, and finally saving the data.

2.2 Fault data preprocessing and dataset creation:
2. Experimental comparison of adding position encoding and input_embedding
2.1 Define positional encoding and input_embedding
\t  
Comparative model:
Model A: Transformer Encoder Layer
Model B: Transformer encoder layer + input_embedding - positional encoding
2.2 Experimental comparison of the Western Reserve University highly classified dataset
(1) Model A:
\t  
Model Evaluation:
Accuracy, Precision, Recall, F1 Score
\t  
(2) Model B:
Model Evaluation:
Accuracy, Precision, Recall, F1 Score
\t  
\t2.3 \tGearbox Bearing Faults at Southeast University - Experimental Comparison of Five-Classification Dataset
(1) Model A:
\t  
Model Evaluation:
Accuracy, Precision, Recall, F1 Score
\t  
(2) Model B:
\t  
\t  
Model Evaluation:
Accuracy, precision, recall, F1 Score
3. Experimental comparison results analysis
Through comparative experiments with two datasets, we have found that the Transformer encoder layer alone achieves satisfactory results in fault signal classification tasks. However, when input encoding and position encoding are added, the classification performance deteriorates, indicating that position encoding does not play a particularly significant role in fault signal classification tasks! In the next session, we will further discuss which core modules in Transformer are crucial for improving classification accuracy.
1. 27 Failure Diagnosis Models for Bearings | The Most Affordable Full Set of Products on the Internet
2. Bearing Failure Diagnosis—Innovative Model Full Set
3. Cross-Attention Innovation Model Full Set for Bearing Failure
Previously Interesting Content:
Python - Kaiser Permanente University (CWRU) Bearing Data Interpretation and Classification Processing
Python Bearing Failure Diagnosis (Part I): Short-Time Fourier Transform STFT
Python Bearing Failure Diagnosis (Part II): Continuous Wavelet Transform CWT
Python Bearing Failure Diagnosis (Part III): Empirical Mode Decomposition EMD
Pytorch-LSTM Bearing Failure One-Dimensional Signal Classification (Part I)
Pytorch-CNN Bearing Failure One-Dimensional Signal Classification (Part II)
Pytorch-Transformer Bearing Failure One-Dimensional Signal Classification (Part III)
Python Bearing Failure Diagnosis (Part IV): Fault Classification Based on EMD-CNN
Python Bearing Failure Diagnosis (Part V): Fault Classification Based on EMD-LSTM
Python Bearing Failure Diagnosis (Part VI): Fault Classification Based on EMD-Transformer
Python Bearing Failure Diagnosis (Part VII): Fault Classification Based on EMD-CNN-LSTM
Python Bearing Failure Diagnosis (Part VIII): Fault Classification Based on EMD-CNN-GRU Parallel Model
Bearing Failure Identification Model Based on FFT + CNN - BiGRU-Attention, Integrating Time-Domain and Frequency-Domain Features Attention
Bearing Failure Identification Model Based on FFT + CNN - Transformer, Integrating Time-Domain and Frequency-Domain Features Integration
Python Bearing Failure Diagnosis (Part IX): Fault Classification Based on VMD+CNN-BiLSTM
Python Bearing Failure Diagnosis (Part X): Fault Classification Based on VMD+CNN-Transformer
Python Bearing Fault Diagnosis (11) Hybrid Fault Classification Based on VMD+CNN-BiGRU-Attenion
Cross Attention Fusion of Time and Frequency Features with FFT + CNN-BiLSTM-CrossAttention for Bearing Fault Identification Model
Cross Attention Fusion of Time and Frequency Features with FFT + CNN-Transformer-CrossAttention for Bearing Fault Identification Model
Introduction to Python Bearing Fault Diagnosis
Python Bearing Fault Diagnosis (13) Super-strong Machine Learning Recognition Model Based on Fault Signal Feature Extraction
Python Bearing Fault Diagnosis (14) Highly Innovative Fault Recognition Model
Python Bearing Fault Diagnosis (15) One-Dimensional Fault Signal Recognition Model Based on CNN-Transformer
Python Bearing Fault Diagnosis (16) Highly Innovative Fault Recognition Model (II)
SCI 1st Region Highly Innovative Bearing Fault Diagnosis Model!
Python Bearing Fault Diagnosis (18) Innovative Diagnosis Model Based on CNN-TCN-Attention
Highly Innovative Bearing Fault Diagnosis Model Based on GADF+Swin-CNN-GAM
Attention Magic Modification | Super-strong Bearing Fault Diagnosis Model!
Bearing Fault—Cross Attention Innovative Model Full Set
Python Bearing Fault Diagnosis (19) Innovative Diagnosis Model Based on Transformer-BiLSTM
END

## Images

![Detailed Explanation of Positional Encoding in Transformer](23.jpg)
![Detailed Explanation of Positional Encoding in Transformer](24.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
