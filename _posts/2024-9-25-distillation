

</head>
<body class="nd-docs">
<div class="nd-pageheader">
 <div class="container">
 <h1 class="lead">
 <nobr class="widenobr">Teacher-Student Training</nobr>
 <!-- <nobr class="widenobr">For DS 4440</nobr> -->
 </h1>
 </div>
</div><!-- end nd-pageheader -->

<div class="container">
<div class="row">
<div class="col justify-content-center text-center">
<h2>An Analysis of "Distilling the Knowledge in a Neural Network"</h2>
<p>Paper Link: <a name="bottou-1990">[1]</a> <a href="https://arxiv.org/pdf/1503.02531.pdf"
  >Geoffrey Hinton, Oriol Vinyals, and Jeff Dean.
  <em>Distilling the Knowledge in a Neural Network</em></a>
  NIPS Deep Learning and Representation Learning Workshop (2015).</p>
<h5>by George Bikhazi and Tijana Cosic</h5>
</div>
</div>
<div class="row">
<div class="col">

<h2>1) Introduction</h2>

<p>
  For our project, we are exploring the topic of teacher-student training,
  or more formally known as knowledge distillation. It deals with the idea
  of transferring knowledge about certain data from a larger, more complex model
  (the "teacher" model) to a smaller, more efficient "student" model. It is
  a beneficial technique used in machine learning for a variety of reasons.
  
  One of the primary advantages of teacher-student training is model
  compression. By distilling knowledge from a larger, complex "teacher"
  model into a smaller, more lightweight "student" model, we can create
  models that are faster and more resource-efficient for deployment on
  devices with limited computational resources, such as mobile phones or
  IoT devices.

  In addition, experiments have shown that student models trained using
  knowledge distillation often achieve better performance in terms of
  its accuracy and efficiency as compared to models trained directly on the
  original dataset. This is in part due to the improved generalization
  that teacher-student training encourages. Instead of learning from the
  hard targets (where the probability of each output is either a 1 or a 0),
  the student model learns from soft targets (a more realistic target
  distribution over all classes where instead of outputs having a
  probability of 0% or 100%, they can have probabilities of 20% and 80%,
  for example.) In teacher-student training, the dataset provides hard
  targets (a single target label) and the teacher provides soft targets
  (a distribution over all labels).

  <br>
  In the original paper, Geoffrey Hinton, Oriol Vinyals, and Jeff Dean conduct
  experiments on the simple MNIST dataset. In our project we explored whether knowledge
  distillation could be applied to more complex datasets, such as CIFAR-10.
</p>

<p>
  <img alt="cifar10 example images" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/cifar_pics.png" style="width:600;height:200px;">
</p>


<h2>2) Paper Summary</h2>
<p>
  Knowledge distillation is a form of model compression, and the original paper
  provides a new technique for doing so while giving an intuition
  for why it works.
</p>

<p>
  It all starts with the softmax function. A neural network outputs a vector of
  logits, z, for each class. The softmax function converts these logits into
  probabilities, q, for each class. A large, successful classification model will
  typically output very high probabilities for the correct class and very low
  probabilities for the incorrect classes. In the context of an image classification
  model, an "image of a BMW [...] may only have a very small chance of being mistaken
  for a garbage truck, but that mistake is still many times more probable than
  mistaking it for a carrot" [1].
</p>

<p>
  The idea is that these probabilities contain a lot of information about the
  relationships between classes that the model has learned. The paper argues that
  this information is not utilized when training a new model from scratch, since
  that involves the new network just looking at one-hot ground truth vectors. 
  The paper suggests that the model could be trained more efficiently if it could
  learn from the probabilities that the teacher model outputs. This is where
  knowledge distillation comes in. The teacher model's probabilities are used as
  soft targets for the student model to learn from. The student model is trained
  to output similar probabilities to the teacher model, rather than just the
  one-hot ground truth vectors. The teacher model's probability distribution
  can also be softened to highlight the differences in the probabilities between
  classes, which can help the student model learn more effectively. This softening
  is done by introducing a concept of <q>temperature</q> to the softmax function:
</p>

<p>
  <img alt="softmax with temperature" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/softmax_with_temp.png" style="width:400;height:200px;">
</p>

<p>The paper suggests using a weighted average of the teacher model's output and the
  ground truth label as the target for the student model.
</p>

<p>
  In our project, we end up using the same techniques as described in the paper,
  but we apply them to the CIFAR-10 dataset instead of MNIST. CIFAR-10 is a more
  complex dataset with larger, richer images.
</p>

<h2>3) Implementation</h2>

<p>
  We had access to several large pre-trained models from the <a href="https://github.com/huyvnphan/PyTorch_CIFAR10">PyTorch_CIFAR10 repo</a>.
  We ended up using the vgg13_bn model as our teacher model. It had 94.22% validation accuracy on CIFAR-10. Here is its architecture:
</p>

<img alt="vgg13_bn architecture" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/teacher.png" style="width:400;height:500px;">

<p>
  <br>
  We set up a very simple student model as a proof of concept. It used the following architecture:
</p>

<img alt="student model architecture" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/student.png" style="width:500;height:400px;">

<p>
  <br>
  Our training loop was set up like so:
</p>

<img alt="training code" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/train.png" style="width:500;height:400px;">

<p>
  <br>
  It is important to note what is being compared in the loss function. The student model's output logits are fed through a softmax
  function with an increased temperature. This is then compared to a weighted average of the teacher model's output (also fed through
  a softmax with increased temperature) and the ground-truth one-hot label.
</p>

<p>Our implementation can be found <a href="https://drive.google.com/drive/folders/1oDBVMREh2TsdqTD1S8uD-AxlWJihYOJv?usp=sharing">here</a>. 
The folder is originally a fork of the <a href="https://github.com/huyvnphan/PyTorch_CIFAR10">PyTorch_CIFAR10 repo</a> to make interacting
with the pre-trained models easy. Our own implementation lives in the teacher_student.ipynb and eval.ipynb files.
</p>

<h2>4) Results</h2>

<p>
  <br>
  We trained a few versions of the student model shown above with different hyperparameters.
  In particular, we trained the student model with alpha=0.0, alpha=0.5, and alpha=1.0.
  Alpha is the weight of the teacher model's output in the loss function. Alpha=0.0 means
  that the student model is only learning from the ground truth labels, while alpha=1.0
  means that the student model is only learning from the teacher model's output. Here are 
  the accuracies of the student models compared with the accuracy of the teacher model:
</p>

<img alt="acc by model" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/acc_by_model.PNG">

<p>student_ht represent the student model with alpha=0.0, aka the model trained solely on hard
  targets (hence the "ht"). student_st represents the student model with alpha=1.0, aka the model
  trained solely on soft targets (hence the "st"). student_mix represents the student model with alpha=0.5,
  aka the model trained on an even mix of soft and hard targets.
</p>

<p>
  We  can see that the student model that was trained on just the hard
  targets performed worse than the models trained on the soft targets and the mix of
  soft and hard targets. This supports the paper's argument that using probabilities
  of all classes for predicting a class is advantageous and provides improved accuracy.   
</p>

<p>
  <br>
  To take a deeper look at the models' accuracies, we created graphs to show the accuracy
  of each class in the CIFAR-10 dataset.  
</p>

<img alt="acc by class teacher" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/acc_by_class_teacher.PNG">
<img alt="acc by class ht" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/acc_by_class_ht.PNG">
<img alt="acc by class st" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/acc_by_class_st.PNG">
<img alt="acc by class mix" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/acc_by_class_mix.PNG">

<p>
  Looking at the graphs for student_st and student_mix, we can see that about half of the classes were more often 
  classified correctly in the soft targets model, and the other half of the classes were 
  more often correctly classified in the model trained on the mix of soft and hard 
  targets.  
</p>

<p>
  It is also interesting to see that the student_st model most closely resembles the teacher model in terms of relative 
  class accuracy. This makes intuitivesense since the student_st model was trained solely on the teacher model's output.
</p>

<p>
  <br>
  Lastly, we created confusion matrices to additionally show what classes the models
  were getting correct/incorrect the most.
</p>

<img alt="conf matrix st" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/conf_matrix_st.PNG">
<img alt="conf matrix mix" class="img-fluid" src="https://raw.githubusercontent.com/gbikhazi20/distillation/refs/heads/main/images/conf_matrix_mix.PNG">

<p>
  We were intrigued to find that part of the mistakes the models were making was that
  they were specifically confusing modes of transportation, including a car, a plane,
  a ship, and a truck. In addition, they were confusing classes in the middle of the 
  matrix, such as birds, dogs, cats, and deers. 
</p>

<h2>5) Conclusion</h2>

<p>
  Our project was mainly experimenting with different ways of training a student model 
  from a pre-trained teacher model in order to determine how advantageous and efficient 
  it is. We trained multiple student models with various alphas in order to see if 
  the number how this would affect accuracy. In the end, we 
  discovered that using soft targets does, in fact, improve the model's performance.
  For future work, we would like to use different pre-trained models as the teacher 
  model to see if this will make a difference when training student models.
</p>

<h3>References</h3>

<p><a name="bottou-1990">[1]</a> <a href="https://arxiv.org/pdf/1503.02531.pdf"
  >Geoffrey Hinton, Oriol Vinyals, and Jeff Dean.
  <em>Distilling the Knowledge in a Neural Network</em></a>
  NIPS Deep Learning and Representation Learning Workshop (2015).
</p>
<p><a name="github-repo">[2]</a> <a href="https://github.com/huyvnphan/PyTorch_CIFAR10"
  >Huy Phan.
  <em>PyTorch_CIFAR10 GitHub repo</em></a>
</p>

<h2>Team Members</h2>
                                                   
<p>Tijana Cosic | cosic.t@northeastern.edu, 
   George Bikhazi | bikhazi.g@northeastern.edu
</p>

  
</div><!--col-->
</div><!--row -->
</div> <!-- container -->

<footer class="nd-pagefooter">
  <div class="row">
    <div class="col-6 col-md text-center">
      <a href="https://ds4440.baulab.info/">About DS 4440: Practical Neural Networks</a>
    </div>
  </div>
</footer>

</body>
<script>
$(document).on('click', '.clickselect', function(ev) {
  var range = document.createRange();
  range.selectNodeContents(this);
  var sel = window.getSelection();
  sel.removeAllRanges();
  sel.addRange(range);
});
// Google analytics below.
window.dataLayer = window.dataLayer || [];
</script>
</html>