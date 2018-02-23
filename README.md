# DeepBindToKeras

Av here. I wrote code to convert DeepBind models to Keras. It was a bit painful, but I am pleased with the result. https://github.com/kundajelab/DeepBindToKeras/blob/master/DeepBind%20to%20Keras.ipynb

A few nice features: as with the original DeepBind prediction code, I automatically take the max over the forward and reverse complement if the model is for DNA sequence. I also handle the case of models having a hidden fully-connected layer and average pooling (average pooling is present for some RNA models).

A few things to note:

- Keras requires all the inputs in a batch to be of the same length (input length can vary between batches, but within a batch the length must be the same). For this reason, my conversion code requires you to specify the input length.
- When the input size is <= int(conv_filter_length\*1.5), the output is exactly identical to the DeepBind output within numerical precision (I did the comparison using the 4 example sequences and 4 models that ship with DeepBind)
- When the input size is > int(conv_filter_length\*1.5), the outputs are similar but there are slight differences due to the peculiar way that DeepBind does padding. When the input is > int(conv_filter_length\*1.5), DeepBind proceeds as follows:
```
--> Break the input into windows of size int(conv_filter_length*1.5)
--> Pad each window with conv_filter_length-1 N's (the choice of conv_filter_length-1 is because it does what are called "full" convolutions, meaning the convolution is applied whenever it partly overlaps with the input; contrast this with "valid" or "same" convolutions).
--> Push each window through the network (which contains global max pooling and sometimes global average pooling)
--> Take the max output over all windows.
```
- I found it strange that DeepBind chooses to pad the windows with N’s when it could instead pad the windows with the actual sequence adjacent to the windows when such sequence is available. I implemented the latter both because it makes more biological sense and because it is more scalable to implement in Keras (otherwise, I would have to pad each window separately; this way, I pad the entire sequence only once and use TimeDistributed Dense layers to achieve the windowing). I confirmed that when the DeepBind code is modified to pad the windows with the actual sequence rather than pad with N’s, the outputs agree within numerical precision.
