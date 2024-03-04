---
title: "Adapters for Low Precision Neural Networks"
excerpt: "A Research into the use of adapters for finetuning quantized neural networks namely binarised neural networts <br/><img src='/images/500x300.png'>"
permalink: /projects/Thesis
collection: projects
---
You can read the full report [here](/files/JohnSuThesis_Adapters.pdf)

# Abstract
On the extreme end of low precision neural networks are binary neural networks (BNN),
where weights and activations are quantized to 1 and -1. BNNs have the potential to run
deep networks on dramatically reduced hardware. However, BNNs often need special training methods making transfer learning difficult. Furthermore, it is cumbersome to require
multiple versions of the same networks for different domains. To alleviate this, adapters are
investigated as a solution to this problem.

Adapters inject additional parameters into the network and are a fraction of the size of the
base network. During finetuning, the weights in the BNN are kept frozen and the adapters are
trained to repurpose the BNN to the new domain. This eliminates the need to have multiple
copies of the same deep network. Instead, adapters can be swapped out to repurpose the base
network to a new domain. Because of their smaller memory footprint, multiple adapters can
be stored for the same cost as a single deep network to cover multiple tasks.

In this project, three points are examined; the effectiveness of adapters compared to
standard transfer learning methods, the training time and finally the size of an adapter needed to maintain high accuracy. Both a serial and parallel adapter approach were considered.

Convolutional based BNNs are examined with the task being focused on image classification.
Using pruning and grouped convolutions to reduce weights, experiments indicate that
adapters that are between 7-12% of the memory size of the base BNN provide a good
compromise between accuracy and low memory footprint. Accuracy of BNNs are only a
few percentage points off standard transfer learning methods while providing faster training
times and flexibility in design approach. This project opens a path to implementing deep
networks on low end hardware via BNNs with adapters being used to tailor the network to
the specific task.

You can read rest [here](/files/JohnSuThesis_Adapters.pdf)