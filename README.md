# chunker
Robust Phrasal Chunking

Definition of the model: we use the baseline method which implements a semi-character RNN to deal with noisy input (method 1), in order to create a character level representation of the word. Firstly, we create a one-hot vector v1 for the first character of the word. Secondly, we create a vector v2 where the index of a character has the count of that character in the word. Finally, we create a one-hot vector v3 for the last character of the word. And the size for each vector is 100 due to the length of the Python builtin “string.printable” which is 100 characters. Moreover, the final character level representation is the concatenation of these three vectors which is a 300 dimensional vector. In the end, we choose to use the first method which is concatenating to the word embedding input to the chunker RNN an input vector that is the character level representation of the word, specifically the 300 dimensional vector mentioned above. The method is given in HW3 Baseline section.

Definition of the implementation: we wrote a new function called prepare_semiChar(seq) which takes a sequence of words as input and outputs a 2D tensor of size sentence length times 300 for a given sentence. Then this 2D tensor of character vectors can then be concatenated with the word embeddings which is a 2D tensor of size sentence length times 128. Thus, we need to change the LSTM input from 128 to 428 (128+300). Also, before outputting the tensors, we use torch.stack to stack all of these 300 dimensional concatenated vectors into a single 2D tensor. Moreover, there is a trick in combining these multi-dimensional tensors which is that the two 2D tensors' alignment is such that one is 37 x 300 and another one is 128 x 37. In order to concatenate them, we need to do transpose one before concatenating them. In addition, the output of the prepare_semiChar needs to be converted to float by using Python's float() function before attempting concatenation. 

Evaluation: After implementing the semi-character RNN, there is substantial boost in performance (about 6%). This simple method has essentially made the model resilient to noise and rather than identifying many misspelled words as <unkown>, they are still recognized and considered by the model.
