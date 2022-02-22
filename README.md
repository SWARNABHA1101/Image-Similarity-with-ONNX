# Image-Similarity-with-ONXX

1)**Open Neural Network Exchange (ONNX)** is an open format built to represent machine learning models. Since it was open-sourced in 2017, ONNX has developed into a standard for AI, providing building blocks for machine learning and deep learning models. ONNX defines a common file format to enable AI developers to use models with various frameworks, tools, runtimes, and compilers, and helps increase the speed of innovation in the artificial intelligence community.</br>

2)All the well-known operations in the Neural Networks world have 
corresponding ONNX operators, for example, `torch.nn.Linear` would have an operator 
which represents a multi-layer perceptron, `torch.nn.Conv2d` would be for the convolution 
operation and so forth. But what happens if a new state-of-the-art models comes up with an 
entirely novel architecture (something like Deformable Convolutions)? How would one go 
about exporting such models into the ONNX format? The answer is, **custom ONNX operators!** </br>
              
![image](https://user-images.githubusercontent.com/84931642/155128535-d5d5732b-560d-49fa-a0d2-459fdcddf868.png) </br>
 ## Why use C++ with ONNX?


<li>Well, not that Python is slower than C++, you’d end up calling the 
same C++ APIs when using Python front end. But, the raison d'être for the C++ front end is 
latency.</li>
<li> Imagine building a self-driving car model that takes half a second to respond to a 
pedestrian on the road. The results would be disastrous.</li>
<li> Furthermore, we can’t perform hardware-level optimizations using Python which we would otherwise do while using C++ like 
strong memory management, using CPU and GPU cores efficiently (multithreading on Python 
is a joke), compiler level optimizations, and so on.</li></br>
<li>We’ll create a novel-model which we’ll call ReductionResNet.First, let’s start with a pretrained
ResNet-18 model from torchvision which has 4 layers (4 layers with a pair of ResBlock
each). Now we would like to extract features from each of the layer and use that as a features 
(rather than only using the output of the last layer) Following is a picture of something we’d like 
to do.</li></br>

![image](https://user-images.githubusercontent.com/84931642/155133211-22da35f5-ebd6-47f9-a86d-40695f221b51.png) </br>
In the above diagram, consider an input image of resolution 416x416 the diagram indicates the 
expected output dimensions of the 4 layers respectively. In the output dimensions, for instance 
for the first layer, the output [1, 64, 104, 104]indicates [B, C, H, W]where B, C, H, 
W are the batch size, channel size, height, and width of the output maps. The height and width 
would change if the input resolution is different, but the number of channels would remain 
constant.

