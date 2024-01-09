---
layout: distill
title: Pruned RNN-T Explained
date:  2024-01-07 16:40:16
description: This blog explains the paper- ``Pruned RNN-T for fast, memory-efficient ASR training''
# tags: formatting links
categories: tutorial
giscus_comments: false

authors:
  - name: Sangeet Sagar
    affiliations:
      name: EML Speech Technology GmbH
# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Paper theme
  - name: Introduction
  - name: Motivations
  - name: Pruned RNN-T
  - name: Experimental Settings
---

## **Paper theme**
RNN-T has a slow and memory-intensive loss function, limiting its use for large vocabularies like Chinese characters. The main idea of Pruned RNN-T is to reduce computational and memory requirements by selectively evaluating the joiner network, leading to faster training and maintained accuracy. Pruning bounds are obtained using a linear joiner network in the encoder and decoder embeddings.

[Source Code](https://github.com/k2-fsa/k2/blob/415fe1f446fffe1d9e7219b5033966294c0b430c/k2/python/k2/rnnt_loss.py#L636)

## **Introduction**
- Three popular E2E models are CTC models, attention-based models, and RNN-T models, with RNN-T being suitable for streaming decoding and doesn’t assume frames are independent, 

- The output of an RNN-T model is a 4-D tensor ($$N$$, $$T$$, $$U$$, $$V$$)  
    - $$N$$ : batch size 
    - $$T$$ : output length 
    - $$U$$ : prediction length 
    - $$V$$ : vocabulary size, which consumes a lot of memory. 
- Removing padding, function merging and half-precision training are approaches to reduce memory usage in RNN-T 
- In this paper RNN-T = RNN-T loss = transducer loss
- The paper uses a Conformer encoder and a stateless decoder in their experiments, not a recurrent encoder and decoder. 

## **Motivations**

<div class="row justify-content-center">
    <div class="col-sm mt-3 mt-md-0" style="max-width: 300px; text-align: center;">
        {% include figure.html path="assets/img/transducer_model.png" title="example image" class="img-fluid rounded z-depth-1 mx-auto" %}
    </div>
</div>

<div class="caption">
    Illustration of a generic transducer model. 
    <a href="https://lorenlugosch.github.io/posts/2020/11/transducer/">Reference</a>
    (Lugosch, 2020)
</div>


**Tokens:**
- The transducer model utilizes tokens, such as the Beginning-Of-Sequence (BOS) token `<s>` placed at `y0`, and a blank token (∅). It has no EOS.

**Equations:**

- **Input Features:**
  - Denoted as: $$\mathbf{x} = \{x_0, x_1, x_2, \ldots, x_{t-1}\} \equiv X_{(T, E)}$$
  - These represent the input features, typically derived from audio or text data.

- **Tokenized Transcript:**
  - Denoted as: $$\mathbf{y} = \{y_0, y_1, y_2, \ldots, y_u\}$$ (with a total of $$U$$ tokens)
  - Represents the tokenized transcript, where each $$y_i$$ corresponds to a token in the output sequence.
  
- **Predictor Network (Decoder):**
  - The predictor network, or decoder, is autoregressive. It takes the previous output as input and generates features used for generating the next output token.

- RNN-T loss computation can be memory and compute-intensive due to the large output shape: $$ N \times T \times U \times V $$.


<div class="row justify-content-center">
    <div class="col-sm mt-3 mt-md-0" style="max-width: 600px; text-align: center;">
        {% include figure.html path="assets/img/Pruned_RNNT_lattice.png" title="example image" class="img-fluid rounded z-depth-1 mx-auto" %}
    </div>
</div>

<div class="caption">
    This illustration shows the lattice in standard RNN-T (right) vs pruned RNN-T (left).
    <a href="https://arxiv.org/pdf/2206.13236.pdf">arXiv</a>
    (Fangjun Kuang, 2022)
</div>

{% details What is a lattice? and why is it important? %}
The lattice represents the log-probs of transition between the time steps and label indices. They are important because they capture the likelihood of transition to a token at a time step.
{% enddetails %}

- Pruned RNN-T limits the token range from $$U$$ to $$S$$ at each time step, reducing output shape to $$(T, S, V)$$. This reduction reduces memory consumption and speeds up training.

## **Pruned RNN-T**
- Pruned RNN-T selectively evaluates the joiner network for specific $(t, u)$ pairs that have a significant impact on the final loss. This is achieved by performing the core recursion of the model twice:
    - First, with a "trivial" joiner network, which is fast to evaluate, to identify important pairs.  
    - Then, the full joiner network is evaluated only for a subset of (t, u) pairs. 
- **Trivial joiner network**:
    The trivial joiner network is a simplified approach to computing the joiner network in the RNN-T model, using matrix multiplication and lookups to efficiently obtain log probabilities for the pruned RNN-T model.
    $$y(t,u) \rightarrow \text{log-probs of vertical transition}$$<br>
    $$ \phi(t,u) \rightarrow \text{log-probs of horizontal transition}$$<br>
    $$L_{enc}(t,u) \rightarrow \text{unnormalized log-probs associated with encoder}$$<br>
    $$L_{dec}(t,u) \rightarrow \text{unnormalized log-probs associated with decoder aka. prediction network}$$<br>
    
    Log probs associated with the joiner is given by

    $$L_{trivial} = L_{enc} + L_{dec} - \log \left( \sum \exp({L_{enc} + L_{dec}}) \right)$$ 
    
    this equation allows us to compute $$y(t,u)$$ and $$\phi(t,u)$$

- **Pruning bounds**: Pruning involves selecting a constant $$S$$ (eg 4 or 5) to limit the evaluation of $$L(t, u, v)$$ for specific $$u$$ indexes within a given $$t$$ index. In the above illustration, the lattice on the right will have $$L(t,u,v)$$ computed for only for $$p_t \leq u < p_t + S$$ indices, while the rest is set to $$-\infty$$.
To compute the **globally optimal pruning bounds** we want to find a sequence of integer pruning bounds $$p=p_0, p_t, ... p_{T-1}$$ that maximizes the total retained probability. It is basically finding positions for pruning in the lattice such that the total probability of retained transition is maximized. Here we are trying to retain the highest probability transition while discarding the less relevant ones.

In the context of the pruned RNN-T model, the quantities $$y′(t, u)$$ and $$\phi′(t, u)$$ represent "occupation counts" within a specific interval, indicating the likelihood of upward and rightward transitions. Lets estimate the retained probability mass for $$S=4$$ (no. of label indices to be evaluated at each time step $$t$$) and $$p_t=2$$ (pruning bound)--

$$
\phi'(t,2) + \phi'(t,3) + \phi'(t,4) + \phi'(t,5) - y'(t,1)
$$

The occupation count $$y'$$ in the above equation represents the probability associated with label index 1 which is red in the above lattice diagram. The subtraction is done to make the calculation more accurate by compensating for the inclusion of some probability mass that should have been pruned out due to lower $$u$$ values.

- **Loss function**: [Source Code](https://github.com/k2-fsa/k2/blob/415fe1f446fffe1d9e7219b5033966294c0b430c/k2/python/k2/rnnt_loss.py#L450) The loss function is a combination of log-probs from trivial joiner network and full joiner network.

$$
L_{smoothed} = (1- \alpha^{lm} - \alpha^{acoustic})L_{trivial} + \alpha^{lm}L_{lm} + \alpha^{acoustic}L_{lm}
$$

*Refer to Section 3.3 in the paper for a more detailed explanation*

## **Experimental Settings**

<table border="1" cellpadding="5">
  <tr>
    <th>Category</th>
    <th>Parameter/Hyperparameter</th>
    <th>Value</th>
  </tr>
  <tr>
    <td rowspan="4">Dataset</td>
    <td>Corpus</td>
    <td>LibriSpeech</td>
  </tr>
  <tr>
    <td>Training Hours</td>
    <td>960 hours</td>
  </tr>
  <tr>
    <td>Test Sets</td>
    <td>test-clean, test-other</td>
  </tr>
  <tr>
    <td>Test Set Speech Duration</td>
    <td>Approximately 5 hours each</td>
  </tr>
  <tr>
    <td rowspan="3">Input Features</td>
    <td>Feature Type</td>
    <td>80-dimension log Mel filter bank</td>
  </tr>
  <tr>
    <td>Window Size</td>
    <td>25 ms</td>
  </tr>
  <tr>
    <td>Window Shift</td>
    <td>10 ms</td>
  </tr>
  <tr>
    <td>Data Augmentation</td>
    <td>SpecAugment Factors</td>
    <td>0.9 and 1.1</td>
  </tr>
  <tr>
    <td rowspan="4">Model Architecture</td>
    <td>Encoder</td>
    <td>Conformer with 12 layers</td>
  </tr>
  <tr>
    <td>Encoder Self-Attention</td>
    <td>8 heads</td>
  </tr>
  <tr>
    <td>Encoder Attention Dim</td>
    <td>512</td>
  </tr>
  <tr>
    <td>Encoder Feed-Forward Dim</td>
    <td>2048</td>
  </tr>
  <tr>
    <td rowspan="2">Decoder</td>
    <td>Stateless decoder with embedding layer and 1-D convolutional layer</td>
    <td> </td>
  </tr>
  <tr>
    <td>Decoder Embedding Dim</td>
    <td>512</td>
  </tr>
  <tr>
    <td>GPUs</td>
    <td>Number of GPUs</td>
    <td>8 NVIDIA V100 32GB GPUs</td>
  </tr>
  <tr>
    <td rowspan="2">Training Strategy</td>
    <td>Pruning Strategy</td>
    <td>Enable pruned loss after convergence of trivial loss</td>
  </tr>
</table>

## Results
- Pruned RNN-T outperforms other implementations in terms of speed and memory efficiency. 
- When comparing Word Error Rates (WERs) on the LibriSpeech test-clean and test-other datasets, the model trained with pruned RNN-T shows slightly better WER performance compared to the model trained with unpruned RNN-T loss. This suggests that pruned RNN-T can achieve comparable or better accuracy in ASR tasks.
<div class="row justify-content-center">
    <div class="col-sm mt-3 mt-md-0" style="max-width: 500px; text-align: center;">
        {% include figure.html path="assets/img/Pruned_RNNT_results.png" title="example image" class="img-fluid rounded z-depth-1 mx-auto" %}
    </div>
</div>
- The memory efficiency of pruned RNN-T allows for the use of larger batch sizes and vocabulary sizes during training, which further contributes to its speed advantage.

