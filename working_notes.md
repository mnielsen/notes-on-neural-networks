# Notes on neural networks

**Working notes, by Michael Nielsen:** These are rough working notes,
written as part of my study of neural networks.  Note that they really
are _rough_, and I've made no attempt to clean them up.  There are
many misunderstandings, misinterpretations, omissions, and outright
errors in the notes.  I make no apology for this, nor do I have any
interest in cleaning them up. As such, I don't advise reading the
notes, and you certainly shouldn't rely on them!

**Williams and Zipser (1989):**
  ([link](http://scholar.google.ca/scholar?cluster=1352799553544912946&hl=en&as_sdt=0,5))
  Provides a gradient-based learning method for recurrent neural
  networks.  
  
  Makes the claim that feedforward networks don't have the "ability to
  store information for later use".  It'd be nice to understand what
  that means.  Obviously there's a trivial sense in which feedforward
  networks can store information based on training data.  Claims that
  backprop requires lots of memory when used with large amounts of
  training data.  I don't understand this claim.
  
  Their model of recurrent neural networks is interesting.  Basically,
  we have a set of neurons, each with an output.  And we have a set of
  inputs to the network.  There is a weight between every pair of
  neurons, and from each input to each neuron.  To compute a neuron's
  output at time $t+1$ we compute the weighted sum of the inputs and
  the outputs at time $t$, and apply the appropriate nonlinear
  function (sigmoid, or whatever).  Note that in order for this
  description to make sense we must specify the behaviour of the
  external inputs over time.  We can incorporate a bias by having an
  external input which is always $1$.
  
  So a recurrent neural network is just like a feedforward network,
  with a weight constraint: the weights in each layer are the same,
  over and over again.  Also, the inputs must be input to every layer
  in the network.
  
  Williams and Zipser take as their supervised training task the goal
  of getting neuron outputs to match certain desired training values
  at certain times.  For instance, you could define a two-neuron
  network that will _eventually_ produce the XOR of the inputs.
  
  They define the total error to be the sum over squares of the errors
  in individual neuron outputs.  And we can then do ordinary gradient
  descent with that error function.  They derive a simple dynamical
  system to describe how to improve the weights in the network using
  gradient descent.

  The above algorithm assumes that the weights in the network remain
  constant for all time.  Williams and Zipser then modify the learning
  algorithm, allowing it to change the weights at each step.  The idea
  is simply to compute the total error at any _given_ time, and then
  to use gradient descent with that error function to update the
  weights.  (Similar to online learning in feedforward networks.)
  
  Williams and Zipser describe a method of _teacher-forcing_,
  modifying the neural network by replacing the output of certain
  neurons by the _desired_ output, for training purposes in later
  steps.
  
  Unfortunately, it is still really unclear to me _why_ one would wish
  to use recurrent neural networks.  Williams and Zipser describe a
  number of examples, but they don't seem terribly compelling to me.
  
  The algorithm in which the weights can change seems very
  non-physiological to me --- it verges on being an unmotivated
  statistical model.  (I doubt that the weights in the brain swing
  around wildly, but I'll bet that the weights found by this algorithm
  can swing around wildly.)  The algorithm in which the weights are
  fixed seems more biological.
  
  Note that Williams and Zipser _do not_ offer any analysis of running
  time for their algorithms, or an understanding of when it is likely
  to work well, and when it is not.  It's very much in the empirical
  let's-see-how-this-works style adopted through much of the neural
  networks literature.
  
  Summing up: the recurrent neural network works by, at each step,
  computing the sigmoid function of the weighted sum of the inputs and
  the previous step's outputs.  Training means specifying a set of
  desired outputs at particular times, and adapting the weights at
  each time-step.  Training works by specifying an error function at
  any given time step, computing the gradient, and updating the
  weights appropriately.

**Compiling to neural networks:** One idea that may be interesting is
  to create compilers which would translate a program written in a
  conventional programming language into a neural network.  I'd be
  especially interested in seeing how this works for AI workhorses
  such as Prolog.  What could we learn from such a procedure?  (1)
  Perhaps we could figure out how to link up multiple neural modules,
  with one or more of the modules coming from the compiler? (2) Maybe
  we could use a learning technique to further improve the performance
  of the compiled network.  Googling doesn't reveal a whole lot,
  although I did find a paper by
  [Thrun](http://scholar.google.ca/scholar?cluster=10518384657895134615&hl=en&as_sdt=0,5)
  where he discusses decompiling, i.e., extracting rules from a neural
  network.  Thrun uses a technique he calls validity-interval
  analysis, basically propagating intervals for inputs and outputs
  forwards and backwards through a network.

**Softmax function:** Suppose $q_j$ is some set of values.  Then we
  define the softmax function by:
  
  $$p_j \equiv \exp(q_j)/\sum_k \exp(q_j).$$
  
  This is a probability distribution, which preserves the order of the
  original values.  You can, for example, take the softmax in the
  final layer of a neural network, taking the weighted sum of inputs
  as the $q_j$ values, and then applying the softmax.  The output from
  the network can then be interpreted as a probability distribution.
