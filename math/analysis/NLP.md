







### SVD Based Methods



The first set are count-based and rely on matrix factorization (e.g. LSA, HAL). 

While these methods effectively leverage global statistical information, they are primarily used to capture word similarities and do poorly on tasks such as word analogy, indicating a sub-optimal vector space structure.





recent, probabilistic
method by [Mikolov et al., 2013] : word2vec. Word2vec is a software
package that actually includes :
- 2 algorithms: continuous bag-of-words (CBOW) and skip-gram. CBOW aims to predict a center word from the surrounding context in terms of word vectors. Skip-gram does the opposite, and predicts the distribution (probability) of context words from a center word.
- 2 training methods: negative sampling and hierarchical softmax. Negative sampling defines an objective by sampling negative examples, while hierarchical softmax defines an objective using an efficient

tree structure to compute probabilities for all the vocabulary.





### Iteration Based Methods - Word2vec





The other set of methods are shallow window-based (e.g. the skip-gram and the CBOW models), which learn word embeddings by making predictions in local context windows. These models demonstrate the capacity to capture
complex linguistic patterns beyond word similarity, but fail to make use of the global co-occurrence statistics.



#### Language Models (Unigrams, Bigrams, etc.)











##### Continuous Bag of Words Model (CBOW)



predict a center word from the surrounding context in terms of word vectors

- Ex: "The cat jumped over the puddle."
- treat {"The", "cat", ’over", "the’, "puddle"} as a context and from these words and attempt to predict or generate the center word "jumped". 



Create an input word matrix $V = \left[ \begin{matrix} \mathbf{v}_1 & \dots & \mathbf{v}_{|\text{Vocab}|}  \end{matrix}\right]\in  \mathbb{R}^{n \times|\text{Vocab}|}$ and an output word matrix $U =\left[ \begin{matrix} \mathbf{u}_1^\top \\ \vdots \\ \mathbf{u}_{|\text{Vocab}|}^\top  \end{matrix}\right]  \in  \mathbb{R}^{|\text{Vocab}| \times n}$, where $n$ is an arbitrary size which defines the size of our embedding space.

The model works in these steps:

- Generate the one hot word vectors for the input context of size $m$: 
  $$
  \mathbf{x}^{(c−m)}, \dots, \mathbf{x}^{(c−1)},\mathbf{x}^{(c+1)},\dots,\mathbf{x}^{(c+m)} \in \mathbb{R}^{|\text{Vocab}|}
  $$
  
  
  
- get the embedded word vectors for the context
  $$
  \mathbf{v}_{(c−m)} = V \mathbf{x}^{(c−m)}, \dots, \mathbf{v}_{(c−1)} =V\mathbf{x}^{(c−1)},\mathbf{v}_{(c+1)} = V \mathbf{x}^{(c+1)},\dots,\mathbf{v}_{(c+m)} =V\mathbf{x}^{(c+m)} \in \mathbb{R}^{n}
  $$
  
- Average these vectors to get 
  $$
  \hat{\mathbf{v}} = \frac{\mathbf{v}_{(c−m)} +\dots+\mathbf{v}_{(c+m)} }{2m} \in \mathbb{R}^{n}
  $$
  
- Generate a score vector 
  $$
  \mathbf{z}=U \hat{\mathbf{v}} \in \mathbb{R}^{|\text{Vocab}|}
  $$
  

  As the dot product of similar vectors is higher, it will push similar words close to each other in order to achieve a high score.

- Turn the scores into probabilities $\hat{\mathbf{y}} = \operatorname{softmax}(\mathbf{z}) \in    \mathbb{R}^{|\text{Vocab}|}$
- We desire the probabilities generated, $\hat{\mathbf{y}} \in    \mathbb{R}^{|\text{Vocab}|}$ to match the true probabilities, $ \mathbf{y}\in    \mathbb{R}^{|\text{Vocab}|}$ , which is the one hot vector of the actual word.




$$
\mathcal{J}(\hat{\mathbf{y}}, \mathbf{y}) = − \sum_{j=1}^{|\text{Vocab}|}y_j \log(\hat{y}_j) = −   \log(\hat{y}_c) =-\log \frac{ e^{\mathbf{u}_c^\top\hat{\mathbf{v}}}}{\sum_{j=1}^{|\text{Vocab}|} e^{\mathbf{u}_j^\top\hat{\mathbf{v}}} }=-{\mathbf{u}_c}^\top\hat{\mathbf{v}}+\log \left(\sum_{j=1}^{|\text{Vocab}|} e^{\mathbf{u}_j^\top\hat{\mathbf{v}}}\right)
$$

Training proceeds in an on-line, stochastic fashion.

Stochastic gradient descent is used to compute gradients for a window and update all relevant word vectors $\mathbf{u}_c$ and $\mathbf{v}_j$



##### Skip-Gram Model

Predicting surrounding context words given a center word:

- Ex: "The cat jumped over the puddle."
- Given the center word "jumped", attempt to predict or generate the surrounding words "The", "cat", "over", "the", "puddle".

 

The setup of the Skip-Gram model is largely the same but we essentially swap our x and y i.e. x in the CBOW are now y and vice-versa. 

The input one hot vector (center word) we will represent with an $\mathbf{x}$ (since there is only one). And the output vectors as $y^{(j)}$. 

$V$ and $U$ is defined to be the same as in CBOW.



this model works in these 6 steps:

- generate the one hot input vector $\mathbf{x} \in  \mathbb{R}^{|\text{Vocab}|} $ of the center word.
- We get our embedded word vector for the center word $ \mathbf{v}_c = V\mathbf{x}  \in \mathbb{R}^{n} $
- Generate a score vector $\mathbf{z}=U \mathbf{v}_c \in \mathbb{R}^{|\text{Vocab}|}$
- Turn the scores into probabilities $\hat{\mathbf{y}} = \operatorname{softmax}(\mathbf{z}) \in    \mathbb{R}^{|\text{Vocab}|}$. Note $\hat{y}_{c-m},\dots ,\hat{y}_{c-1},\hat{y}_{c+1},\dots ,\hat{y}_{c+m}$ are the probabilities of observing each context word.
- We desire our probability vector generated to match the true probabilities which is $\mathbf{y}^{(c−m)}, \dots, \mathbf{y}^{(c−1)},\mathbf{y}^{(c+1)},\dots,\mathbf{y}^{(c+m)} $, the one hotvectors of the actual output.



Only one probability vector $\hat{\mathbf{y}}$ is computed. Skip-gram treats each context word equally : the models computes the probability for each word of appearing in the context independently of its distance to the center word (all output words are assumed to be completely independent):

$$
\mathcal{J}\left(\hat{\mathbf{y}}, \mathbf{y}^{(c−m)}, \dots, \mathbf{y}^{(c−1)},\mathbf{y}^{(c+1)},\dots,\mathbf{y}^{(c+m)}\right)= -\log \prod_{j=0,\, j\neq m}^{2m} \hat{y}_{c-m+j} \\ =-\log \prod_{j=0,\, j\neq m}^{2m} \frac{ \exp \left(  \mathbf{u}_{c-m+j}^\top\mathbf{v}_c \right) }{\sum_{j=1}^{|\text{Vocab}|}  \exp\left({\mathbf{u}_j}^\top\mathbf{v}_c\right) }=- \sum_{j=0,\, j\neq m}^{2m} {\mathbf{u}_{c-m+j}}^\top\mathbf{v}_c+2m\,\log \left(\sum_{j=1}^{|\text{Vocab}|} \exp\left({\mathbf{u}_j}^\top\mathbf{v}_c\right) \right)
$$
With this objective function, we can compute the gradients with respect to the unknown parameters and at each iteration update them via Stochastic Gradient Descent. 

Note that
$$
\mathcal{J} \left(\hat{\mathbf{y}}, \mathbf{y}^{(c−m)}, \dots, \mathbf{y}^{(c−1)},\mathbf{y}^{(c+1)},\dots,\mathbf{y}^{(c+m)} \right) =
\sum_{j=0,\, j\neq m}^{2m} \mathcal{J} \left(\hat{\mathbf{y}}, \mathbf{y}^{(c−m+j)}\right)
$$
where $ \mathcal{J} \left(\hat{\mathbf{y}}, \mathbf{y}^{(c−m+j)}\right)$ is the cross-entropy between the probability vector $\hat{\mathbf{y}}$ and the one-hot vector $\mathbf{y}^{(c−m+j)}$.





##### Negative Sampling

Loss functions J for CBOW and Skip- Gram are expensive to compute because of the softmax normalization, where we sum over all  $|\text{Vocab}|$ scores!

Any update we do or evaluation of the objective function would take $\mathcal{O}(|\text{Vocab}|)$ time which is in the millions.



For every training step, instead of looping over the entire vocabulary, we can just sample several negative examples! We "sample" from a noise distribution (Pn(w)) whose probabilities match the ordering of the frequency of the vocabulary.



Mikolov et al. present Negative Sampling in Distributed Representations of Words and Phrases and their Compositionality.

While negative sampling is based on the Skip-Gram model, it is in fact optimizing a different objective. 



Consider a pair $(w, c)$ of word and context. Denote by $P(D = 1|w, c)$ the probability that $(w, c)$ came from the corpus data. Correspondingly, $P(D = 0|w, c)$ will be the probability that (w, c) did not come from the corpus data.

model $P(D = 1|w, c)$ for $(w,c) \in D$ with the sigmoid function:
$$
P(D = 1|w, c) = \sigma( \mathbf{u}_{w}^\top\mathbf{v}_c) =\frac{1}{1+\exp \left( - \mathbf{u}_{w}^\top\mathbf{v}_c\right)}
$$
model $P(D = 0|w, c)$ for $(w,c) \in \tilde{D}$ with the sigmoid function:
$$
P(D = 0|w, c) = 1- \sigma( \mathbf{u}_{w}^\top\mathbf{v}_c) =\frac{1}{1+\exp \left(  \mathbf{u}_{w}^\top\mathbf{v}_c\right)}
$$
Note that $\tilde{D }$ is a "false" or "negative" corpus, where we would have sentences like "stock boil fish is toy". Unnatural sentences that should get a low probability of ever occurring. We can generate  $\tilde{D }$ on the fly by randomly sampling this negative corpus from the word bank.



build a new objective function that tries to maximize the probability of a word and context being in the corpus data if it indeed is, and maximize the probability of a word and context not being in the corpus data if it indeed is not.

minimizing the corresponding negative log likelihood:
$$
\mathcal{J} = - \sum_{(w,c) \in D} \log \frac{1}{1+\exp \left( - \mathbf{u}_{w}^\top\mathbf{v}_c\right)} - \sum_{(w,c) \in \tilde{D}} \log\frac{1}{1+\exp \left(  \mathbf{u}_{w}^\top\mathbf{v}_c\right)}
$$






- For skip-gram, our new objective function for observing the context word $c − m + j$ given the center word $c$ would be
  $$
  -  \log  \sigma( \mathbf{u}_{c − m + j}^\top\mathbf{v}_c)- \sum_{k=1}^K  \log \, \sigma( -\mathbf{u}_{k}^\top\mathbf{v}_c)
  $$
  where $K$ is the number of examples generated by negative sampling.

  Compare this with the regular softmax loss for skip-gram
  $$
  -   \mathbf{u}_{c-m+j}^\top\mathbf{v}_c+ \,\log \left(\sum_{j=1}^{|\text{Vocab}|} \exp\left({\mathbf{u}_j}^\top\mathbf{v}_c\right) \right)
  $$
  
- For CBOW, the objective function for observing the center word $\mathbf{u}_c$ given the context vector $\hat{\mathbf{v}} = \frac{\mathbf{v}_{(c−m)} +\dots+\mathbf{v}_{(c+m)} }{2m} $ would be
  $$
  -  \log  \sigma( \mathbf{u}_{c}^\top\hat{\mathbf{v}})- \sum_{k=1}^K  \log \, \sigma( - \mathbf{u}_{k}^\top \hat{\mathbf{v}})
  $$
  To compare with the regular softmax loss for CBOW
  $$
  -{\mathbf{u}_c}^\top\hat{\mathbf{v}}+\log \left(\sum_{j=1}^{|\text{Vocab}|} \exp{\left( \mathbf{u}_j^\top\hat{\mathbf{v}}\right)}\right)
  $$





In the above formulation, $ \{\mathbf{u}_k|k = 1 \dots K\}$ are sampled from $P_n(w)$. $P_n(w)$ that makes the best approximation seems to be the Unigram Model raised to the power of $3/4$. Why $3/4$? 

Here’s an example that might help gain some intuition:
"is": 0.93/4 = 0.92
"Constitution": 0.093/4 = 0.16
"bombastic": 0.013/4 = 0.032

"Bombastic" is now 3x more likely to be sampled while "is" only went up marginally.





##### Hierarchical Softmax

Mikolov et al. also present hierarchical softmax as a much more efficient alternative to the normal softmax. In practice, hierarchical softmax tends to be better for infrequent words, while negative sampling works better for frequent words and lower dimensional vectors.



Hierarchical Softmax uses a binary tree where leaves are the words (there is a unique path from root to leaf). The probability of a word being the output word is defined as the probability of a random walk from the root to that word's leaf. Computational cost becomes O(log(|V|)) instead of O(|V|).



To train the model, our goal is still to minimize the negative log likelihood − log P(w|wi). But instead of updating output vectors per word, we update the vectors of the nodes in the binary tree that are in the path from root to leaf node.

The speed of this method is determined by the way in which the binary tree is constructed and words are assigned to leaf nodes.

Mikolov et al. use a binary Huffman tree, which assigns frequent words shorter paths in the tree.





### Global Vectors for Word Representation (GloVe)



GloVe consists of a weighted least squares model that trains on global word-word co-occurrence counts and thus makes efficient use of statistics. The model produces a word vector space with meaningful sub-structure. It shows state-of-the-art performance on the word analogy task, and outperforms other current methods on several word similarity tasks.



Using global statistics to predict the probability of word $j$ appearing in the context of word $i$ with a least squares objective





###### Co-occurrence Matrix:

- $X$: word-word co-occurrence matrix
- $X_{ij}$: number of times word $j$ occur in the context of word $i$
- $X_i = \sum_k X_{ik}$: the number of times any word $k$ appears in the context of word $i$ 
- $Pij = P(w_j|w_i) = \frac{X_{ij}} {X_i}$: the probability of $j$ appearing in the context of word $i$



Previously, we use softmax to compute the probability of word $j$ appears in the context of word $i$:
$$
Q_{ij} = \frac{ \exp\left(\mathbf{u}_j^\top \mathbf{v}_i\right)}{\sum_{w=1}^{|\text{Vocab}|} \exp\left(\mathbf{u}_w^\top \mathbf{v}_i\right) }
$$


Training proceeds in an on-line, stochastic fashion, but the implied global cross-entropy loss can be calculated as:
$$
\mathcal{J} = - \sum_{i \, \in \,\text{corpus}} \sum_{j \, \in \,\text{context}(i)} \log \, Q_{ij}
$$
As the same words  $i$ and $j$ can appear multiple times in the corpus, it is more efficient to first group together the same values for $i$ and $j$:
$$
\mathcal{J} = - \sum_{i =1}^{|\text{Vocab}|} \sum_{j =1}^{|\text{Vocab}|} X_{ij}\log \, Q_{ij}
$$
One significant drawback of the cross-entropy loss is that it requires the distribution Q to be properly normalized, which involves the expensive summation over the entire vocabulary.

Instead, we use a least square objective in which the normalization factors in P and Q are discarded:
$$
\mathcal{J} =   \sum_{i =1}^{|\text{Vocab}|} X_{i} \left(\sum_{j =1}^{|\text{Vocab}|} \left(\hat{P}_{ij} -\hat{Q}_{ij}\right)^2\right)
$$
where $\hat{P}_{ij}=X_{ij}$ and $\hat{Q}_{ij}=\exp\left(\mathbf{u}_j^\top \mathbf{v}_i\right)$ are the unnormalized distributions.

This formulation introduces a new problem – $X_{ij}$ often takes on very large values and makes the optimization difficult. An effective change is to minimize the squared error of the logarithms of $\hat{P}$ and $\hat{Q}$:
$$
\mathcal{J} =   \sum_{i =1}^{|\text{Vocab}|} X_{i} \left(\sum_{j =1}^{|\text{Vocab}|} \left(\log \hat{P}_{ij} -\log\hat{Q}_{ij}\right)^2\right) = \sum_{i =1}^{|\text{Vocab}|}  \sum_{j =1}^{|\text{Vocab}|}   X_{i}\left(\mathbf{u}_j^\top \mathbf{v}_i -\log X_{ij} \right)^2
$$
Another observation is that the weighting factor Xi is not guaranteed to be optimal. Instead, we introduce a more general weighting function $f$, which we are free to take to depend on the context word as well:

$$
\mathcal{J} = \sum_{i =1}^{|\text{Vocab}|}  \sum_{j =1}^{|\text{Vocab}|}   f(X_{ij})\left(\mathbf{u}_j^\top \mathbf{v}_i -\log X_{ij} \right)^2
$$




In conclusion, the GloVe model efficiently leverages global statistical information by training only on the nonzero elements in a word-word co-occurrence matrix, and produces a vector space with meaningful sub-structure. It consistently outperforms word2vec on the word analogy task, given the same corpus, vocabulary, window size, and training time. It achieves better results faster, and also obtains the best results irrespective of speed.



### Problem Formulation

Most NLP extrinsic tasks can be formulated as classification tasks. For instance, given a sentence, we can classify the sentence to have positive, negative or neutral sentiment. 

Similarly, in **named-entity recognition (NER)**, given a context and a central word, we want to classify the central word to be one of many classes. For the input, "Jim bought 300 shares of Acme Corp. in 2006", we would like a classified output "[Jim]<sub>Person</sub> bought 300 shares of [Acme Corp.]<sub>Organization</sub> in [2006]<sub>Time</sub>."

For such problems, we typically begin with a training set of the form: $\{\mathbf{x}^{(i)},\mathbf{y}^{(i)}\}_1^N$



where $\mathbf{x}^{(i)}$ is a $d$-dimensional word vector generated by some word embedding technique and $\mathbf{y}^{(i)}$ is a $C$-dimensional one-hot vector which indicates the labels we wish to eventually predict (sentiments, other words, named entities, buy/sell decisions, etc.).



In typical machine learning tasks, we usually hold input data and target labels fixed and train weights using optimization techniques
(such as gradient descent, L-BFGS, Newton’s method, etc.). In NLP applications however, we introduce the idea of retraining the input word vectors when we train for extrinsic tasks.



the objective function using a simple linear classifier with a single word vector $\mathbf{x}$:
$$
- \sum_{i=1}^N  \log  \left(\frac{ \exp\left( W_{k(i)}\, \mathbf{x}^{(i)} \right) }{ \sum_{c=1}^C \exp\left( W_c \,\mathbf{x}^{(i)}\right) } \right) +\lambda \; \sum_{l=1}^{C\cdot d + |\text{Vocab}|\cdot d} \theta_l^2
$$


where $k(i)$ is a function that returns the correct class index for example $\mathbf{x}^{(i)}$.

if we consider training both, model weights ($W$), as well word vectors ($\mathbf{x}$),  the total number of parameters would be as many as $C\cdot d + |\text{Vocab}|\cdot d $ 



In reality, this is hardly done because of the nature of natural languages. Natural languages tend to use the same word for very different meanings and we typically need to know the context of the word usage to discriminate between meanings.





For instance, if you were asked to explain to someone what "to sanction" meant, you would immediately realize that depending on the context "to sanction" could mean "to permit" or "to punish".

In most situations, we tend to use a sequence of words as input to the model. A sequence is a central word vector preceded and succeeded by context word vectors. The number of words in the context is also known as the context window size and varies depending on the problem being solved. 

Generally, narrower window sizes lead to better performance in syntactic tests while wider windows lead to better performance in semantic tests.







### Language Models

Language models compute the probability of occurrence of a number of words in a particular sequence. The probability of a sequence of $m$ words $ \{w_1, \dots, w_m\}$ is denoted as $P(w_1, \dots, w_m)$. 

Since the number of words coming before a word, $w_i$, varies depending on its location in the input document, $P(w_1, \dots, w_m)$ is usually conditioned on a
window of $n$ previous words rather than all previous words:
$$
P(w_1, \dots, w_m) = \prod_{i=1}^m  P(w_i |w_1, \dots, w_{i-1}) \approx \prod_{i=1}^m  P(w_i |w_{i-n}, \dots, w_{i-1})
$$


$P(w_i |w_{i-n}, \dots, w_{i-1})$ focuses on making predictions based on a fixed window of context (i.e. the $n$ previous words) used to predict the next word. 

But how long should the context be? In some cases, the window of past consecutive n words may not be suf-
ficient to capture the context. 



For instance, consider the sentence "As the proctor started the clock, the students opened their ____". If the window only conditions on the previous three words "the students opened their", the probabilities calculated based on the corpus may
suggest that the next word be "books" - however, if n had been large
enough to include the "proctor" context, the probability might have
suggested "exam".











## Recurrent Neural Networks


$$
\mathbf{a}^{\langle t \rangle} = \tanh \left(W_{\mathbf{a}\mathbf{a}}\mathbf{a}^{\langle t-1 \rangle}+  W_{\mathbf{a}\mathbf{x}}\mathbf{x}^{\langle t \rangle} + \mathbf{b}_{\mathbf{a}} \right)  \\

\hat{\mathbf{y}}^{\langle t \rangle} = \operatorname{softmax}  \left(W_{\mathbf{y}\mathbf{a}}\mathbf{a}^{\langle t \rangle} + \mathbf{b}_{\mathbf{y}} \right)  \\
$$


The categorical cross-entropy loss of a sequnce of size $T$:
$$
\mathcal{J} = -\frac{1}{T}  \sum_{t=1}^{T} \sum_{j=1}^{|\text{Vocab}|} y_j^{\langle t \rangle} \log\,\hat{y}_j^{\langle t \rangle}
$$



$$
\frac{d \mathcal{J}}{d W_{\mathbf{a}\mathbf{a}}} =  \sum_{t=1}^{T} \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d \mathbf{a}^{\langle t \rangle}}{d W_{\mathbf{a}\mathbf{a}}}
$$




$$
\begin{align*}
 \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d\mathbf{a}^{\langle t \rangle}}{dW_{\mathbf{aa}}} 
 
 & = \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \left( \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial W_{\mathbf{aa}}} +\frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}} \frac{d \mathbf{a}^{\langle t-1 \rangle}}{d W_{\mathbf{aa}}}\right) \\

 &= \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial W_{\mathbf{aa}}} + \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\left(\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial W_{\mathbf{aa}}}+ \frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial \mathbf{a}^{\langle t-2 \rangle}} \frac{d \mathbf{a}^{\langle t-2 \rangle}}{d W_{\mathbf{aa}}} \right) \\
 
& = \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial W_{\mathbf{aa}}} + \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}}+ \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial \mathbf{a}^{\langle t-2 \rangle}} \left(\frac{\partial \mathbf{a}^{\langle t-2 \rangle}}{\partial W_{\mathbf{aa}}}+ \frac{\partial \mathbf{a}^{\langle t-2 \rangle}}{\partial \mathbf{a}^{\langle t-3 \rangle}}\frac{d \mathbf{a}^{\langle t-3 \rangle}}{d W_{\mathbf{aa}}} \right)  \\
 
& = \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}} + \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}}+ \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial \mathbf{a}^{\langle t-2 \rangle}} \frac{\partial \mathbf{a}^{\langle t-2 \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}} +\frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial \mathbf{a}^{\langle t-2 \rangle}}   \frac{\partial \mathbf{a}^{\langle t-2 \rangle}}{\partial \mathbf{a}^{\langle t-3 \rangle}} \left(\frac{d \mathbf{a}^{\langle t-3 \rangle}}{d W_{\mathbf{aa}}} \right)  \\

&= \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}\frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}} 
+ \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-1 \rangle}}\frac{\partial \mathbf{a}^{\langle t-1 \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}}
+ \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t-2 \rangle}}\frac{\partial \mathbf{a}^{\langle t-2 \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}} + \dots 

+ \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}} \frac{\partial \mathbf{a}^{\langle 1 \rangle} }{\partial W_{\mathbf{a}\mathbf{a}}}  \\

&=\sum_{k=1}^t \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{\mathbf{a}\mathbf{a}}}
\end{align*}
$$



Note for $t>k$:
$$
\frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} = \prod_{j=k+1}^t  \frac{\partial \mathbf{a}^{\langle j \rangle}}{\partial \mathbf{a}^{\langle j-1 \rangle}}
$$
where $\mathbf{z}^{\langle j \rangle} = W_{\mathbf{a}\mathbf{a}}\mathbf{a}^{\langle j-1 \rangle}+  W_{\mathbf{ax}}\mathbf{x}^{\langle j \rangle} + \mathbf{b}_{\mathbf{a}}$



Note:
$$
\prod_{j=k+1}^t  \frac{\partial \mathbf{a}^{\langle j \rangle}}{\partial \mathbf{a}^{\langle j-1 \rangle}} =  \prod_{j=k+1}^t  \operatorname{diag}[\tanh'(\mathbf{z}^{\langle j \rangle})] W_{\mathbf{aa}}  
$$




Note that the norm of $\operatorname{diag}[\tanh'(\mathbf{z} )]$ or $\operatorname{diag}[\sigma'(\mathbf{z} )]$ can only be as large as $1$.



Similarly, we have 
$$
\frac{d \mathcal{J}}{d W_{\mathbf{ax}}} =  \sum_{t=1}^{T} \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d \mathbf{a}^{\langle t \rangle}}{d W_{\mathbf{ax}}}
$$


and
$$
\begin{align*}
 \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d\mathbf{a}^{\langle t \rangle}}{dW_{\mathbf{ax}}} 
 
&=\sum_{k=1}^t \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{\mathbf{ax}}}
\end{align*}
$$



$$
\frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{ij}}=   \operatorname{diag}[\tanh'(\mathbf{z}^{\langle k \rangle})] \frac{\partial \mathbf{z}^{\langle k \rangle}}{\partial W_{ij}} \\= \operatorname{diag}[\tanh'(\mathbf{z}^{\langle k \rangle})]  \left[\begin{matrix}0 \\ \vdots \\x_j^{\langle k \rangle} \\ \vdots \\0 \end{matrix}\right] \leftarrow i\text{ th element } \\ =
 \left[\begin{matrix}0 \\ \vdots \\ \left(1-\left(a_i^{\langle k \rangle} \right) ^2  \right)x_j^{\langle k \rangle} \\ \vdots \\0 \end{matrix}\right] \leftarrow i\text{ th element }
$$

$$
\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{ij}} \\
=\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \left( \prod_{j=k+1}^t  \operatorname{diag}[\tanh'(\mathbf{z}^{\langle j \rangle})] W_{\mathbf{aa}} \right) \operatorname{diag}[\tanh'(\mathbf{z}^{\langle k \rangle})]  \left[\begin{matrix}0 \\ \vdots \\x_j^{\langle k \rangle} \\ \vdots \\0 \end{matrix}\right] \\
=\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \left( \prod_{j=k}^t  \operatorname{diag}[\tanh'(\mathbf{z}^{\langle j \rangle})]  \right) \left[ {W_{\mathbf{aa}}}^{t-k} \right]_{i\text{th column}} x_j^{\langle k \rangle}
$$


So
$$
\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{\mathbf{ax}}} = \left( \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{z}^{\langle k \rangle}} \right)^\top {\mathbf{x}^{\langle k \rangle}}^\top
$$

$$
\frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d\mathbf{a}^{\langle t \rangle}}{d W_{\mathbf{ax}}}
=
\sum_{k=1}^t \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{\mathbf{ax}}}
 = \sum_{k=1}^t  \left( \frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle k \rangle}}  \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{z}^{\langle k \rangle}} \right)^\top {\mathbf{x}^{\langle k \rangle}}^\top
$$
When $t-k$ is sufficiently large, the gradient can be very large or very small.

- once the gradient value grows extremely large, it causes an overflow (i.e. `NaN`) which is easily detectable at runtime; this issue is called the Gradient Explosion Problem. 

- When the gradient value goes to zero, however, it can go undetected while drastically reducing the learning quality of the model for far-away words in the corpus; this issue is called the Vanishing Gradient Problem. Due to vanishing gradients, we don't know whether there is no dependency between steps t and t + n in the data, or we just cannot capture the true dependency due to this issue. 
  - Problem arises from how the model gets structured.
  - 

Gradient updates due to the error observed at step $t$ can hardly be affected by ${\mathbf{x}^{\langle k \rangle}}$ if  $t-k$ is sufficiently large ( ${W_{\mathbf{aa}}}^{t-k} $ )








$$
\frac{d \mathcal{J}}{d \mathbf{x}^{\langle l \rangle}} =  \sum_{t=l}^{T} \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d \mathbf{a}^{\langle t \rangle}}{d \mathbf{x}^{\langle l \rangle}}
$$
For $t>l$:
$$
\begin{align*}
\frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}}  \frac{d \mathbf{a}^{\langle t \rangle}}{d \mathbf{x}^{\langle l \rangle}} &=  \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}}  \left( \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial\mathbf{a}^{\langle t-1 \rangle}} \dots \frac{\partial \mathbf{a}^{\langle l+1 \rangle}}{\partial\mathbf{a}^{\langle l \rangle}} \right)  \frac{d \mathbf{a}^{\langle l \rangle}}{d \mathbf{x}^{\langle l \rangle}} \\
 &= \frac{\partial \mathcal{J}}{\partial \mathbf{a}^{\langle t \rangle}} \left(\prod_{j=l+1}^{t}  \frac{\partial \mathbf{a}^{\langle j \rangle}}{\partial\mathbf{a}^{\langle j-1 \rangle}}  \right) W_{\mathbf{a}\mathbf{x}}
 
 \end{align*}
$$







# Recursive Formulation for Simple RNN


$$
\begin{align*}
\mathbf{a}^{\langle t \rangle}&= \tanh \left(\mathbf{z}^{\langle t \rangle} \right) = \tanh \left(W_{\mathbf{aa}}\mathbf{a}^{\langle t-1 \rangle}+  W_{\mathbf{a}\mathbf{x}}\mathbf{x}^{\langle t \rangle} + \mathbf{b}_{\mathbf{a}} \right)  \\

\hat{\mathbf{y}}^{\langle t \rangle} &= \operatorname{softmax}  \left(W_{\mathbf{y}\mathbf{a}}\mathbf{a}^{\langle t \rangle} + \mathbf{b}_{\mathbf{y}} \right)  

\end{align*}
$$
For $t \geq k$, given $\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}$ (use a matrix to maintian $\left[\begin{matrix}  \left(\frac{\partial \mathcal{J}^{\langle 1 \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}} \right)^\top  & \dots & \left(\frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T \rangle}} \right)^\top  \end{matrix}\right]$)
$$
\begin{align*}
\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial  W_{\mathbf{aa},k}} &=  \frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{z}^{\langle k \rangle}}  \frac{ \partial \mathbf{z}^{\langle k \rangle}}{\partial  W_{\mathbf{aa},k}}  =  \left(\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \; \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle k \rangle} \right) \right]  \right) ^\top {\mathbf{a}^{\langle k-1 \rangle}}^\top
\\
\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k-1 \rangle}} &= \frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{z}^{\langle k \rangle}}  \frac{ \partial \mathbf{z}^{\langle k \rangle}}{\partial \mathbf{a}^{\langle k-1 \rangle}} = \frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}} \;  \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle k \rangle} \right) \right]  \; W_{\mathbf{aa}}
\end{align*}
$$

$$
\frac{d \mathcal{J}}{d W_{\mathbf{aa}}} =  \sum_{t=1}^{T} \frac{ d \mathcal{J}^{\langle t \rangle} } {d W_{\mathbf{aa}}}
$$

$$
\frac{ d \mathcal{J}^{\langle t \rangle} } {d W_{\mathbf{aa}}}=\sum_{k=1}^{t} \frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial  W_{\mathbf{aa},k}} =  \sum_{k=1}^{t}\left(\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \;  \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle k \rangle} \right) \right]   \right) ^\top {\mathbf{a}^{\langle k-1 \rangle}}^\top
$$

similarly
$$
\frac{ d \mathcal{J}^{\langle t \rangle} } {d W_{\mathbf{ax}}}=\sum_{k=1}^{t} \frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial  W_{\mathbf{ax},k}} =  \sum_{k=1}^{t}\left(\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \;  \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle k \rangle} \right) \right]   \right) ^\top {\mathbf{x}^{\langle k \rangle}}^\top
$$


For $\mathbf{x}^{\langle k \rangle}$, if $t-k$ is sufficiently large, $\frac{\partial  \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  $ involves multiplication of repetition of ${W_{\mathbf{aa}}} $ and succession of $ \;  \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle i \rangle} \right) \right]$ and thus could be very small 


$$
\begin{align*}
\frac{d \mathcal{J}}{d W_{\mathbf{aa}}}  &= \sum_{t=1}^{T}\frac{d \mathcal{J}^{\langle t \rangle}} {d W_{\mathbf{aa}}}
=
\sum_{t=1}^{T}\sum_{k=1}^t \frac{\partial \mathcal{J}^{\langle t \rangle}} {\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial W_{\mathbf{aa}}}
 = \sum_{t=1}^{T}\sum_{k=1}^t  \left( \frac{\partial \mathcal{J}^{\langle t \rangle}} {\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{z}^{\langle k \rangle}}\right)^\top {\mathbf{a}^{\langle k-1 \rangle}}^\top
 
 
 \\& =

\begin{matrix} \left( \frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T \rangle}}  \frac{\partial \mathbf{a}^{\langle T \rangle}}{\partial \mathbf{z}^{\langle T \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-1 \rangle} \right)^\top  
 
& +  \left( \frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T -1\rangle}}  \frac{\partial \mathbf{a}^{\langle T-1 \rangle}}{\partial \mathbf{z}^{\langle T-1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-2 \rangle} \right)^\top 

&  +  \left( \frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T -2\rangle}}  \frac{\partial \mathbf{a}^{\langle T-2 \rangle}}{\partial \mathbf{z}^{\langle T-2 \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-3 \rangle} \right)^\top 

& \dots & +  \left( \frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}}  \frac{\partial \mathbf{a}^{\langle 1 \rangle}}{\partial \mathbf{z}^{\langle 1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle 0 \rangle} \right)^\top \\

&+ \left( \frac{\partial \mathcal{J}^{\langle T-1 \rangle}}{\partial \mathbf{a}^{\langle T -1\rangle}}  \frac{\partial \mathbf{a}^{\langle T-1 \rangle}}{\partial \mathbf{z}^{\langle T-1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-2 \rangle} \right)^\top


&  + \left( \frac{\partial \mathcal{J}^{\langle T-1\rangle}}{\partial \mathbf{a}^{\langle T -2\rangle}}  \frac{\partial \mathbf{a}^{\langle T-2 \rangle}}{\partial \mathbf{z}^{\langle T-2 \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-3 \rangle} \right)^\top 
& \dots    & +  \left( \frac{\partial \mathcal{J}^{\langle T-1 \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}}  \frac{\partial \mathbf{a}^{\langle 1 \rangle}}{\partial \mathbf{z}^{\langle 1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle 0 \rangle} \right)^\top \\
 
 && +\left( \frac{\partial \mathcal{J}^{\langle T-2\rangle}}{\partial \mathbf{a}^{\langle T -2\rangle}}  \frac{\partial \mathbf{a}^{\langle T-2 \rangle}}{\partial \mathbf{z}^{\langle T-2 \rangle}} \right)^\top \left( \mathbf{a}^{\langle T-3 \rangle} \right)^\top 
 
 & \dots    & + \left( \frac{\partial \mathcal{J}^{\langle T-2 \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}}  \frac{\partial \mathbf{a}^{\langle 1 \rangle}}{\partial \mathbf{z}^{\langle 1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle 0 \rangle} \right)^\top \\
 
 \\&&& \vdots 
 \\&&  & \dots    & + \left( \frac{\partial \mathcal{J}^{\langle 1 \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}}  \frac{\partial \mathbf{a}^{\langle 1 \rangle}}{\partial \mathbf{z}^{\langle 1 \rangle}} \right)^\top \left( \mathbf{a}^{\langle 0 \rangle} \right)^\top 
\end{matrix}


\\  &= \sum_{t=1}^{T} \left(\sum_{k=t}^T   \frac{\partial \mathcal{J}^{\langle k \rangle}} {\partial \mathbf{a}^{\langle t \rangle}} \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{z}^{\langle t \rangle}}  \right)^\top{\mathbf{a}^{\langle t-1 \rangle}}^\top

=  \sum_{t=1}^{T} \left[\left(\sum_{k=t}^T   \frac{\partial \mathcal{J}^{\langle k \rangle}}{\partial \mathbf{a}^{\langle t \rangle}}   \right)   \frac{\partial \mathbf{a}^{\langle t \rangle}}{\partial \mathbf{z}^{\langle t \rangle}}\right]^\top{\mathbf{a}^{\langle t-1 \rangle}}^\top 

\end{align*}
$$




Given $\left[\begin{matrix}  \left(\frac{\partial \mathcal{J}^{\langle 1 \rangle}}{\partial \mathbf{a}^{\langle 1 \rangle}} \right)^\top  & \dots & \left(\frac{\partial \mathcal{J}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T \rangle}} \right)^\top  \end{matrix}\right]$



The 1st iteration, $t=T$
$$
d \leftarrow {\frac{\partial  \mathcal{J}^{\langle T \rangle} }{\partial \mathbf{a}^{\langle T \rangle}}}^\top
$$
2nd iteration, $t=T-1$:


$$
d \leftarrow \left(\frac{\partial  \mathcal{J}^{\langle T \rangle} }{\partial  \mathbf{a}^{\langle T-1 \rangle}}+ \frac{\partial  \mathcal{J}^{\langle T-1 \rangle} }{\partial \mathbf{a}^{\langle T-1 \rangle}} \right)^\top 
=
 \left(d_{\text{prev}}  \frac{\partial \mathbf{a}^{\langle T \rangle}}{\partial \mathbf{z}^{\langle T \rangle}}  \frac{ \partial \mathbf{z}^{\langle T \rangle}}{\partial \mathbf{a}^{\langle T-1 \rangle}} +\frac{\partial  \mathcal{J}^{\langle T-1 \rangle} }{\partial \mathbf{a}^{\langle T-1 \rangle}}\right)^\top 
=   {W_{\mathbf{aa}}}^\top \;   \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle T\rangle} \right) \right]   \; d_{\text{prev}} + {\frac{\partial  \mathcal{J}^{\langle T-1 \rangle} }{\partial \mathbf{a}^{\langle T-1 \rangle}}}^\top
$$


3rd iteration, $t=T-2$:
$$
d \leftarrow \left(\frac{\partial  \mathcal{J}^{\langle T \rangle} }{\partial  \mathbf{a}^{\langle T-2 \rangle}}+ \frac{\partial  \mathcal{J}^{\langle T-1 \rangle} }{\partial \mathbf{a}^{\langle T-2 \rangle}}  + \frac{\partial  \mathcal{J}^{\langle T-2 \rangle} }{\partial \mathbf{a}^{\langle T-2 \rangle}}  \right)^\top =
 \left(d_{\text{prev}}  \frac{\partial \mathbf{a}^{\langle T-1 \rangle}}{\partial \mathbf{z}^{\langle T -1\rangle}}  \frac{ \partial \mathbf{z}^{\langle T -1\rangle}}{\partial \mathbf{a}^{\langle T-2 \rangle}} +\frac{\partial  \mathcal{J}^{\langle T-1 \rangle} }{\partial \mathbf{a}^{\langle T-2 \rangle}}\right)^\top =   {W_{\mathbf{aa}}}^\top \;   \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle T-1 \rangle} \right) \right]  \; d_{\text{prev}} + {\frac{\partial  \mathcal{J}^{\langle T-2 \rangle} }{\partial \mathbf{a}^{\langle T-2 \rangle}}}^\top
$$
...



initialize $d_{\text{prev}}  \leftarrow 0$

iterate over $t = T,\dots, 1$
$$
d\leftarrow   {W_{\mathbf{aa}}}^\top \;    \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle t+1 \rangle} \right) \right] \; d_{\text{prev}}  + \left(\frac{\partial \mathcal{J}^{\langle t \rangle}}{\partial \mathbf{a}^{\langle t \rangle}} \right)^\top

\\
dW_{\mathbf{aa}}\leftarrow  dW_{\mathbf{aa}}+   \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle t \rangle} \right) \right]  \;d \; \left(  \mathbf{a}^{\langle t -1\rangle}\right)^\top


\\
dW_{\mathbf{ax}}\leftarrow  dW_{\mathbf{ax}}+    \operatorname{diag}\left[ \tanh'\left( \mathbf{z}^{\langle t \rangle} \right) \right]  \; d \; \left(  \mathbf{x}^{\langle t\rangle}\right)^\top
$$


[code](https://raw.githubusercontent.com/justinjiajia/coursera-deep-learning-specialization/master/C5%20-%20Sequence%20Models/Week%201/Dinosaur%20Island%20--%20Character-level%20language%20model/utils.py)

[examples](https://colab.research.google.com/drive/1h9WzozOkgwZ7mm4hlsVqNlo_vBLl54kg?authuser=1#scrollTo=YsaPkvUq01S3)

```python
def rnn_step_backward(dy, gradients, parameters, x, a, a_prev):
    
    gradients['dWya'] += np.dot(dy, a.T)
    gradients['dby'] += dy
    da = np.dot(parameters['Wya'].T, dy) + gradients['da_next'] # backprop into h
    daraw = (1 - a * a) * da # backprop through tanh nonlinearity
    gradients['db'] += daraw
    gradients['dWax'] += np.dot(daraw, x.T)
    gradients['dWaa'] += np.dot(daraw, a_prev.T)
    gradients['da_next'] = np.dot(parameters['Waa'].T, daraw)
    return gradients

def rnn_backward(X, Y, parameters, cache):
    # Initialize gradients as an empty dictionary
    gradients = {}
    
    # Retrieve from cache and parameters
    (y_hat, a, x) = cache
    Waa, Wax, Wya, by, b = parameters['Waa'], parameters['Wax'], parameters['Wya'], parameters['by'], parameters['b']
    
    # each one should be initialized to zeros of the same dimension as its corresponding parameter
    gradients['dWax'], gradients['dWaa'], gradients['dWya'] = np.zeros_like(Wax), np.zeros_like(Waa), np.zeros_like(Wya)
    gradients['db'], gradients['dby'] = np.zeros_like(b), np.zeros_like(by)
    gradients['da_next'] = np.zeros_like(a[0])
    

    for t in reversed(range(len(X))):
        dy = np.copy(y_hat[t])
        dy[Y[t]] -= 1
        gradients = rnn_step_backward(dy, gradients, parameters, x[t], a[t], a[t-1])
    
    return gradients, a
```










##### Solution to the Exploding & Vanishing Gradients

simple and practical heuristic to solve these problems.

- To solve the problem of exploding gradients, Thomas Mikolov first introduced a simple heuristic solution that clips gradients to a small number whenever they explode. That is, whenever they reach a certain threshold, they are set back to a small number.



- To solve the problem of vanishing gradients, we introduce two techniques. 
  - The first technique is that instead of initializing $ W_{\mathbf{a}\mathbf{a}}$ randomly, start off from an identity matrix initialization.
  - The second technique is to use the Rectified Linear Units (ReLU). The derivative for the ReLU is either 0 or 1. This way, gradients would flow through the neurons whose derivative is $1$ without getting attenuated while propagating back through time-steps.



#### Long Short-term Memory Cell


$$
\begin{align*} \Gamma_f^{ \langle t \rangle} &=  \sigma \left(\boldsymbol{\gamma}_f^{ \langle t \rangle} \right)= \sigma  \left(W_f\left[ \begin{matrix} \mathbf{a}^{\langle t-1 \rangle}\\ \mathbf{x}^{\langle t \rangle}  \end {matrix} \right]  + \mathbf{b}_f \right) \\

\Gamma_i^{ \langle t \rangle} &=  \sigma \left(\boldsymbol{\gamma}_i^{ \langle t \rangle} \right) =  \sigma  \left(W_i\left[ \begin{matrix} \mathbf{a}^{\langle t-1 \rangle}\\ \mathbf{x}^{\langle t \rangle}  \end {matrix} \right]  + \mathbf{b}_i \right)

\\
\tilde{\mathbf{c}}^{\langle t \rangle} & = \tanh \left(  \mathbf{z}^{\langle t \rangle}\right) = \tanh  \left(W_c \left[ \begin{matrix} \mathbf{a}^{\langle t-1 \rangle}\\ \mathbf{x}^{\langle t \rangle}  \end {matrix} \right]  + \mathbf{b}_c \right)
\\
 \mathbf{c}^{\langle t \rangle} &= \Gamma_f^{ \langle t \rangle}\circ \mathbf{c}^{\langle t-1 \rangle} + \Gamma_i^{ \langle t \rangle}  \circ \tilde{\mathbf{c}}^{\langle t \rangle}

\\

\Gamma_o^{ \langle t \rangle} &=  \sigma \left(\boldsymbol{\gamma}_o^{ \langle t \rangle} \right) = \sigma  \left(W_o \left[ \begin{matrix} \mathbf{a}^{\langle t-1 \rangle}\\ \mathbf{x}^{\langle t \rangle}  \end {matrix} \right] + \mathbf{b}_o \right) = \sigma  \left( W_{o,\mathbf{a}} \mathbf{a}^{\langle t-1 \rangle} +  W_{o,\mathbf{x}}  \mathbf{x}^{\langle t \rangle} + \mathbf{b}_o \right)

\\

 \mathbf{a}^{\langle t \rangle}  &=  \Gamma_o^{ \langle t \rangle} \circ  \tanh \left( \mathbf{c}^{\langle t \rangle}  \right) 

\\

\hat{\mathbf{y}}^{\langle t \rangle} &= \textrm{softmax}(W_{\mathbf{y}} \mathbf{a}^{\langle t \rangle} + \mathbf{b}_{\mathbf{y}})

\end{align*}
$$

$$
\mathcal{J} = -\frac{1}{T}  \sum_{t=1}^{T} \sum_{j=1}^{|\text{Vocab}|} y_j^{\langle t \rangle} \log\,\hat{y}_j^{\langle t \rangle}
$$





##### LSTM Recursive Formulation



For $t\geq k$, given $\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}} $ and $\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}} $ (when $t=k$, $\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}} =0$)
$$
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_o^{\langle k \rangle}} = \frac{\partial \mathcal{J}^{\langle k \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \Gamma_o^{\langle k \rangle}}  \frac{\partial \Gamma_o^{\langle k \rangle}} {\partial \boldsymbol{\gamma}_o^{\langle k \rangle}} 

=\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}   \operatorname{diag}\left[  \tanh \left( \mathbf{c}^{\langle k \rangle}  \right)  \right] \; \operatorname{diag}\left[ \sigma'\left( \boldsymbol{\gamma}_o^{\langle k \rangle} \right)  \right]
$$

$$
\begin{align*}
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_f^{\langle k \rangle}} &=
\left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}  +  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \frac{\partial  \mathbf{a}^{\langle k \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}   \right)  \frac{\partial \mathbf{c}^{\langle k \rangle}} {\partial \Gamma_f^{\langle k \rangle}} \frac{\partial \Gamma_f^{\langle k \rangle}} {\partial \boldsymbol{\gamma}_f^{\langle k \rangle}} 
\\
&= \left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}   + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \operatorname{diag}\left[ \Gamma_o^{\langle k \rangle}   \right]   \;\operatorname{diag}\left[ \tanh' \left( \mathbf{c}^{\langle k \rangle}  \right)    \right]  \right) \; \operatorname{diag}\left[ \mathbf{c}^{\langle k-1 \rangle}  \right]  
\; \operatorname{diag}\left[ \sigma'\left( \boldsymbol{\gamma}_f ^{\langle k \rangle} \right)  \right]
\end{align*}
$$

$$
\begin{align*}
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_i^{\langle k \rangle}} &= 



\left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}  +  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \frac{\partial  \mathbf{a}^{\langle k \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}   \right) \frac{\partial \mathbf{c}^{\langle k \rangle}} {\partial \Gamma_i^{\langle k \rangle}}  \frac{\partial \Gamma_i^{\langle k \rangle}} {\partial \boldsymbol{\gamma}_i^{\langle k \rangle}} \\

&= \left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}   + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \operatorname{diag}\left[ \Gamma_o^{\langle k \rangle}   \right]   \;\operatorname{diag}\left[ \tanh' \left( \mathbf{c}^{\langle k \rangle}  \right)    \right] \right)

 \; \operatorname{diag}\left[ \tilde{\mathbf{c}}^{\langle k \rangle}  \right]  
\; \operatorname{diag}\left[ \sigma'\left( \boldsymbol{\gamma}_i ^{\langle k \rangle} \right)  \right]
\end{align*}
$$

$$
\begin{align*}
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{z}^{\langle k \rangle}} &=


\left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}  +  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \frac{\partial  \mathbf{a}^{\langle k \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}    \right)  \frac{\partial \mathbf{c}^{\langle k \rangle}} {\tilde{\mathbf{c}}^{\langle k \rangle} }\frac{ \tilde{\mathbf{c}}^{\langle k \rangle} } {\partial \mathbf{z}^{\langle k \rangle}} 

\\  

&= \left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}+\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \operatorname{diag}\left[ \Gamma_o^{\langle k \rangle}   \right]   \;\operatorname{diag}\left[ \tanh' \left( \mathbf{c}^{\langle k \rangle}  \right)    \right] \right) \; \operatorname{diag}\left[ \Gamma_i ^{\langle k \rangle} \right]  \;\operatorname{diag}\left[ \tanh' \left( \mathbf{z}^{\langle k \rangle}  \right)    \right]

\end{align*}
$$




$$
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial W_{o,k}}    & = \frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_o^{\langle k \rangle}}  \frac{\partial \boldsymbol{\gamma}_o^{\langle k \rangle}} {\partial  W_{o,k}} = 
\left(

\frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_o^{\langle k \rangle}} 
 
 \right)^\top \left[ \begin{matrix} \mathbf{a}^{\langle k-1 \rangle}\\ \mathbf{x}^{\langle k \rangle}  \end {matrix} \right] ^\top
 \\
 \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial W_{i,k}}    & = \frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_i^{\langle k \rangle}}  \frac{\partial \boldsymbol{\gamma}_i^{\langle k \rangle}} {\partial  W_{i,k}} = 
\left(

\frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_i^{\langle k \rangle}} 
 
 \right)^\top \left[ \begin{matrix} \mathbf{a}^{\langle k-1 \rangle}\\ \mathbf{x}^{\langle k \rangle}  \end {matrix} \right] ^\top
 
  \\
 \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial W_{f,k}}    & = \frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_f^{\langle k \rangle}}  \frac{\partial \boldsymbol{\gamma}_f^{\langle k \rangle}} {\partial  W_{f,k}} = 
\left(

\frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_f^{\langle k \rangle}} 
 
 \right)^\top \left[ \begin{matrix} \mathbf{a}^{\langle k-1 \rangle}\\ \mathbf{x}^{\langle k \rangle}  \end {matrix} \right] ^\top
 
   \\
 \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial W_{c,k}}    & = \frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial  \mathbf{z}^{\langle k \rangle}}  \frac{\partial \mathbf{z}^{\langle k \rangle}} {\partial  W_{c,k}} = 
\left(

\frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial  \mathbf{z}^{\langle k \rangle}} 
 
 \right)^\top \left[ \begin{matrix} \mathbf{a}^{\langle k-1 \rangle}\\ \mathbf{x}^{\langle k \rangle}  \end {matrix} \right] ^\top
$$

$$
\begin{align*}
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k-1 \rangle}}
&= \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_o^{\langle k \rangle}}  \frac{\partial  \boldsymbol{\gamma}_o^{\langle k  \rangle} }{\partial  \mathbf{a}^{\langle k-1 \rangle} } + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_f^{\langle k \rangle}}  \frac{\partial  \boldsymbol{\gamma}_f^{\langle k  \rangle} }{\partial  \mathbf{a}^{\langle k-1 \rangle} } + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \boldsymbol{\gamma}_i^{\langle k \rangle}}  \frac{\partial  \boldsymbol{\gamma}_i^{\langle k  \rangle} }{\partial  \mathbf{a}^{\langle k-1 \rangle} } + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{z}^{\langle k \rangle}}  \frac{\partial \mathbf{z}^{\langle k \rangle}}{\partial  \mathbf{a}^{\langle k-1 \rangle} }


\end{align*}
$$

$$
\begin{align*}
\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k-1 \rangle}} &=

\left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}} +  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}} \frac{\partial \mathbf{a}^{\langle k \rangle}}{\partial \mathbf{c}^{\langle k \rangle}} \right) \frac{\partial \mathbf{c}^{\langle k \rangle}}{ \partial \mathbf{c}^{\langle k-1 \rangle}} \\
&=  \left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}} +  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \; \operatorname{diag}\left[ \Gamma_o^{\langle k\rangle}  \right]  \; \operatorname{diag} \left[ \tanh'\left( \mathbf{c} ^ {\langle k\rangle}\right) \right]  \right)  \; \operatorname{diag}\left[ \Gamma_f^{\langle k \rangle}  \right]  

\end{align*}
$$








Gradient
$$
\frac{d \mathcal{J}}{d W_f} =  \sum_{t=1}^{T} \frac{ d \mathcal{J}^{\langle t \rangle} } {d W_f}
$$

$$
\frac{d \mathcal{J}^{\langle t \rangle} } {d W_{f,\mathbf{x}}}= \sum_{k=1}^t  \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial W_{f,k, \mathbf{x}}}  = \sum_{k=1}^t\left(

\frac{\partial \mathcal{J}^{\langle t \rangle}}  {\partial \boldsymbol{\gamma}_f^{\langle k \rangle}} 
 
 \right)^\top {\mathbf{x}^{\langle k \rangle} }^\top \\
 = \sum_{k=1}^t\left(\frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{c}^{\langle k \rangle}}   + \frac{\partial \mathcal{J}^{\langle t \rangle} }{\partial \mathbf{a}^{\langle k \rangle}}  \operatorname{diag}\left[ \Gamma_o^{\langle k \rangle}   \right]   \;\operatorname{diag}\left[ \tanh' \left( \mathbf{c}^{\langle k \rangle}  \right)    \right]  \right)^\top  \; \operatorname{diag}\left[ \mathbf{c}^{\langle k-1 \rangle}  \right]  
\; \operatorname{diag}\left[ \sigma'\left( \boldsymbol{\gamma}_f ^{\langle k \rangle} \right)  \right] \;  {\mathbf{x}^{\langle k \rangle} }^\top
$$





Gradients are well-behaved along the $c$ path but not the $h$ path. Luckily LSTMs learn to rely mostly on c for long-term memory. 







#### Attention: in equations



- We have encoder hidden states $\mathbf{h}_1,\dots, \mathbf{h}_N \in \mathbb{R}^h$

- On timestep $t$, we have decoder hidden state $\mathbf{s}_t \in \mathbb{R}^h$

- We get the attention scores (a.k.a. energies) $\mathbf{e}^t$ for this step:
  $$
  \mathbf{e}^t=\left[\begin{matrix} \mathbf{s}_t^{\top}\mathbf{h}_1  ,\; \dots,\; \mathbf{s}_t^{\top}\mathbf{h}_N\end{matrix}\right] \in \mathbb{R}^{N}
  $$

- We take softmax to get the attention distribution for this step (this is a probability distribution and sums to 1)
  $$
  \boldsymbol{\alpha}^t= \operatorname{softmax}(\mathbf{e}^t) \in \mathbb{R}^N
  $$
  
- We use $\boldsymbol{\alpha}$ to take a weighted sum of the encoder hidden states to get the attention output $\mathbf{a}_t$
  $$
  \mathbf{a}_t = \sum_{i=1}^N \alpha^t \mathbf{h}_i \in  \mathbb{R}^h
  $$
  
- Finally we concatenate the attention output $\mathbf{a}_t$ with the decoder hidden state and proceed as in the non-attention seq2seq $\left[\begin{matrix}\mathbf{a}_t  \\\mathbf{s}_t\end{matrix}\right] \in \mathbb{R}^{2h}$

  

More general definition of attention:

- Given a set of vector values, and a vector query, attention is a technique to compute a weighted sum of the values, dependent on the query.

>My understanding
>
>kind of
>
> s successively queries who, what, where ... so as to form a valid sentence with complete semantic meanings
>
>

We sometimes say that the query attends to the values.

- For example, in the seq2seq + attention model, each decoder hidden state (query) attends to all the encoder hidden states (values).
- Intuition:
  - The weighted sum is a ***selective summary*** of the information
    contained in the values, where the query determines which
    values to focus on.
  -  Attention is a way to obtain a ***fixed-size representation of an
    arbitrary set of representations*** (the values), dependent on
    some other representation (the query).





##### Attention variants

There are several ways you can compute $\mathbf{e} \in \mathbb{R}^{N}$ from $\mathbf{h}_1,\dots, \mathbf{h}_N \in \mathbb{R}^{d_1}$ and $\mathbf{s} \in \mathbb{R}^{d_2}$:

- Basic dot-product attention:  ${e}_i=\mathbf{s}^{\top}\mathbf{h}_i \in \mathbb{R}$. Note: this assumes $d_1=d_2$
  
- Multiplicative attention: ${e}_i=\mathbf{s}^{\top}W\mathbf{h}_i \in \mathbb{R}$, where $W \in \mathbb{R}^{d_2 \times d_1}$ is a weight matrix
- Additive attention: ${e}_i=\mathbf{v}^{\top}\tanh (W_1 \mathbf{h}_i+W_2\mathbf{s}) \in \mathbb{R}$, where $W_1 \in \mathbb{R}^{d_3 \times d_1}$ and $W_2 \in \mathbb{R}^{d_3 \times d_2}$ are weight matrices and $\mathbf{v} \in \mathbb{R}^{d_3}$ is a weight vector.
  - $d_3$ (the attention dimensionality) is a hyperparameter



More information: “Deep Learning for NLP Best Practices”, Ruder, 2017. http://ruder.io/deep-learning-nlp-best-practices/index.html#attention
“Massive Exploration of Neural Machine Translation Architectures”, Britz et al, 2017, https://arxiv.org/pdf/1703.03906.pdf







### Self-Attention 

Recall: Attention operates on queries, keys, and values. 

- We have some queries  $\mathbf{q}_1, \mathbf{q}_2,\dots, \mathbf{q}_T $.  Each qury is $\mathbf{q}_i \in \mathbb{R}^{d}$ .  (similar to what was denoted by $\mathbf{s}$ above)
- We have some keys $\mathbf{k}_1, \mathbf{k}_2,\dots, \mathbf{k}_T $. Each key is $\mathbf{k}_i \in \mathbb{R}^{d}$    (simiar to what was denoted by $\mathbf{h}$ above)
- We have some values $\mathbf{v}_1, \mathbf{v}_2,\dots, \mathbf{v}_T $.. Each value is $\mathbf{v}_i \in \mathbb{R}^{d}$ (denoted by $\mathbf{a}$ above)
- The number of queries can differ from the number of keys and values in practice.



In self-attention, the queries, keys, and values are drawn from the same source.

- For example, if the output of the previous layer is $\mathbf{x}_1, \dots, \mathbf{x}_T $., (one vec per word), we could let $\mathbf{v}_i = \mathbf{k}_i = \mathbf{q}_i = \mathbf{x}_i$ (that is, use the same vectors for all of them!)

- The (dot product) self-attention operation is as follows:

  - Compute key-query affinities: $e_{ij}=\mathbf{q}_i^{\top}\mathbf{k}_j$
  - Compute attention weights from affinities (softmax): $\alpha_{ij} = \frac{\exp(e_{ij})}{\sum_{j'}\exp(e_{ij'})} $
  - Compute outputs as weighted sum of values: $\operatorname{output}_i =\sum_j \alpha_{ij} \mathbf{v}_j$

  



Self-attention has a few issues

- First, self-attention is an operation on sets. It has no inherent notion of order. So self-attention doesn't build in order information
  - We need to encode the order of the sentence in our keys, queries, and values.
  - Represent each sequence index $i \in \{1,2,\dots,T\}$ as a vector of dimension $d$

- No nonlinearities for deep learning magic! It’s all just weighted averages 
  - Easy fix: apply the same feedforward network to each self- attention output.
- Need to ensure we don’t “look at the future” when predicting a sequence  (in decoder)
  - At every timestep, we could change the set of keys and queries to include only past words. (Inefficient
  - Look-ahead Mask: to enable parallelization, we ***mask out attention to future words*** by setting attention scores to $− \infty$. i.e., $e_{ij}=\begin{cases} \mathbf{q}_i^{\top}\mathbf{k}_j \quad \text{if }  i>j \\ -\infty \quad \; \,\text{if } i\leq j \end{cases}$





### Transformer model

