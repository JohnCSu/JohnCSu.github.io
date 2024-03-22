---
title: "Torch DE A Pytorch Based PINN Framework"
excerpt: "A Framework for Physics Informed Neural networks with Pytorch Syntax <br/><img src='/images/projects/PINN Gallery.gif'>"
permalink: /projects/TorchDE
collection: projects
---
Here's a small project I've beeen working on: **Torch DE**: a framework for PINN training via pytorch. Keeping the same pytorch flavour, Torch DE helps cut down on a lot of the boilerplate code around PINNs while still giving users the flexibility in making changes if they wish.

You can find the project [here](https://github.com/JohnCSu/torch_DE/tree/main)

<a href = "/images/2024-02-29-Introduction-to-Physics-Informed-Neural-Networks/Test.gif"  >
<img src ="/images/2024-02-29-Introduction-to-Physics-Informed-Neural-Networks/Test.gif">
</a>

# Motivation
<hr>

 I decided to start this project even though there are many PINN frameworks out there such as DeepXDE, and Nvidia Modulus/Sym. While these are great libraries, and starting points to learn PINNs, many of these frameworks I find have the same problems (which is often a result of their strengths):
1. Abstraction is such that it can be too timeconsuming to implement a custom feature (These packages tend to be end-to-end)
2. They stray away from standard pytorch syntax, or any deep learning library for that matter, making it time consuming to learn new syntax

Furthermore, research papers in PINNs tends to be custom made and specific to the researchers chosen problem. This makes code brittle and difficult for other research to reproduce/combine/extend the results. As such, I have, in the best of my abiltity, tried to create Torch DE to avoid these issues such that so that the code sticks to a similar feel to pytorch (and by extension pytorch lightning) while making PINNs more accesable and reproducible.


As you can see from the simplified workflow, there a few steps to consider when setting up a PINN. In Torch DE, I have tried to make each module independant, meaning one can mix and match different modules together as needed. For example, if you are focusing on weighting methods, you can keep everything as is except and just change the weighting method. 

# What Can It Do?
## Strings references over indices management
Torch DE uses (probably an overuse) of dictionaries and strings to reference and manage much of the output that a PINN generates. This reduces the amount of indexing the user has to keep track of making code more readiable and less prone to a error.

```python
from torch_DE.continuous import DE_Getter
#SET UP DERIVATIVES
PINN = DE_Getter(net)
PINN.set_vars(['x','t'],['u'])
PINN.set_derivatives(['u_x','u_t'])
PINN.set_deriv_method('AD')
net = net.cuda()

for i in range(100_000):
    output = PINN.calculate(x)
    # Torch DE relies heavily on dictionarys and string referencing
    out = output['all']
    u,u_x,u_t = out['u'],out['u_x'],out['u_t']

    ...

```

## Geometry
Torch DE comes with a very basic 2D geometry builder made from shapely. THe advantage of this is that you can generate points from somewhat complicated geometry such as in the cylinder or heatsink examples:
Image of them

Note only is generating points inside the domain easier, but it is also easier to generate points along boundaries for boundary conditions.

```python
from torch_DE.geometry.shapes import Domain2D,Rectangle,Cricle
(xmin,xmax),(ymin,ymax) = (0,1), (0,0.41)
domain = Rectangle(((xmin,ymin),(xmax,ymax) ),'corners')
domain = Domain2D(base = domain)

hole = Circle((0.2,0.2),r = 0.05,num_points= 512)
domain.remove(hole,names= ['Cylinder'])

sampled_points = domain.generate_points(2000)
```


<br/>
<a href = '/images/projects/SamplingGeom.jpg'>
    <img src='/images/projects/SamplingGeom.jpg'>
</a>


## Pytorch Flavor
I have tried to keep Torch DE as close to pytorch syntax as close as possible. After setting up, the training loop is  This makes it easier for users to adjust and modify code. This also means that Torch DE is completely compatiable with pytorch lightning or other tools such as weights and biases!
```python

...

for i in range(30_000):
    #Troch DE
    output = PINN.calculate(x)

    ls = losses.calculate(x,output)

    IC_loss,BC_loss,res_loss = ls['Initial Condition'],ls['Periodic'],ls['Residual']

    loss = IC_loss+ res_loss + BC_loss

    #Standard Pytorch Stuff
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    LR_sch.step()
    
```

## Modular and Reproducibilty
Modules of Torch DE are designed to be independent from one another as much as possible. This allows users to swap and discard which modules they find useful or need to modify. The modularity  makes code easier to share and reproduce as only specific components need to be modified instead of the whole pipeline! It should also be easier to reproduce papers (Example the [R3 sampler by Daw et al](https://arxiv.org/abs/2207.02338) can be added  very easily to the workflow as we keep close to pytorch syntax)

```python
...

for i in range(30_000):
    #Troch DE
    output = PINN.calculate(x)

    ls = losses.calculate(x,output)

    IC_loss,BC_loss,res_loss = ls['Initial Condition'],ls['Periodic'],ls['Residual']

    loss = IC_loss+ res_loss + BC_loss

    #R3 Sampler added in
    res = losses.loss_groups['Residual']['collocation points']['convection'](data,output)
    x['collocation points'] = R3_sampler(x['collocation points'],res)

    #Standard Pytorch Stuff
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    LR_sch.step()
    
```
## Examples
See below for some PINNs I have made with Torch DE! 
<br/>
<a href = '/images/projects/PINN Gallery.gif'>
    <img src='/images/projects/PINN Gallery.gif'>
</a>
# To Do
Currently, there's a lot to do to get the project to a state I am happy with:

1. Visualisation Module (it's seriously annoying plotting torch tensors!!)
2. Validation and error tracking with real data
3. Easy time sampling
4. Discrete PINNs (Convolutional and Graph based networks)
5. Operater Learning (e.g. Deep O Nets)
6. Geometry/Meshing import allowing 3D sampling of points (This is definitely a nice to have) 

# Limitations
There are some limitations with Torch DE listed below (as of 18/03/2024):
1. Networks with buffers (such as batch normalisation or layer normalisation) will not work.
2. Multi-GPU support isn't tested and will likely not be implemented in the forseeable future.
