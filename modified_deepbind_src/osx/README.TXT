Av here. This is a version of the deepbind code containing the modification
described in https://github.com/kundajelab/DeepBindToKeras/blob/master/README.md.
With this modification, the results on the example sequences are:

D00210.001  D00120.001  D00410.003  D00328.003
7.059108    -0.187212   -0.649447   -0.026180
-0.172609   4.295672    0.666795    -0.910773
-0.140683   0.181295    5.885349    -0.026180
-0.174985   -0.152521   -0.379695   17.682623

--------------
DEEPBIND v0.11
--------------

The deepbind command-line executable can be used to score DNA/RNA sequences
according to any RBP/TF model listed in the DeepBind web repository:

   http://tools.genes.toronto.edu/deepbind

For each input sequence, the deepbind executable scores each subsequence
of a pre-determined length (e.g. 20) and returns only the maximum or the
average over these per-position scores.

Larger scores indicated stronger binding. The scores themselves are on an 
arbitrary scale, and vary from model to model due to variation in the 
quality of training data for different proteins.


EXAMPLE
-------

To generate predictions with DeepBind, you need two things:

  1) a list of model IDs, and
  2) a list of DNA/RNA sequences.

The file example.ids contains 4 example model IDs, one
on each line, reproduced here:

   D00210.001 # RBFOX1 (RNAcompete)
   D00120.001 # MBNL1  (RNAcompete)
   D00410.003 # GATA3  (SELEX)
   D00328.003 # CTCF   (SELEX)

The file example.seq contains 4 example sequences, which
were chosen such that the nth sequence scores highly for
the nth model. The file example.seq is reproduced here:

   AGGUAAUAAUUUGCAUGAAAUAACUUGGAGAGGAUAGC
   AGACAGAGCUUCCAUCAGCGCUAGCAGCAGAGACCAUU
   GAGGTTACGCGGCAAGATAA
   TACCACTAGGGGGCGCCACC

To generate 16 predictions (4 models, 4 sequences), run
the deepbind executable as follows:

   % deepbind example.ids < example.seq
   D00210.001   D00120.001   D00410.003   D00328.003
    7.451420    -0.166146    -0.408751    -0.026180
   -0.155398     4.113817     0.516956    -0.248167
   -0.140683     0.181295     5.885349    -0.026180
   -0.174985    -0.152521    -0.379695    17.682623

To see details of each ID, use the --dump-info flag:

   % deepbind --dump-info example.ids
   ID          Protein  Type  Species       Family   Class  Experiment  ...
   D00210.001  RBFOX1   RBP   Homo sapiens  RRM             RNAcompete  ...
   D00120.001  MBNL1    RBP   Homo sapiens  Znf             RNAcompete  ...
   D00410.003  GATA3    TF    Homo sapiens  GATA            SELEX       ...
   D00328.003  CTCF     TF    Homo sapiens  C2H2 ZF         SELEX       ...



CHANGES v0.1 -> v0.11
---------------------

- Fixed bug where last position in input sequence was 
  not evaluated for a score; suggested by Irene Kaplow.

- Added --window-size and --average flags based on feedback.

