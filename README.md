# Cones
Unofficial implementation of the <a href='https://arxiv.org/abs/2303.05125'>Cones: Concept Neurons in Diffusion Models for Customized Generation</a> paper.

# Instructions for use:
Configure the provided colab notebook with your Dreambooth dataset. Set an appropriate threshold (activation_thresh) as per the number of steps in the cones_call, then run cones_call to generate concept neuron masks.
After concept masks have been computed, reload image pipeline back with pretrained weights (generating masks alters the weights), pass dictionaries of masks to the image pipeline in cones_inference to apply masks and generate images with the neuron masks.

AFAIK the paper does not provide any threshold values, so this is up for experimentation. I have found anything on the lower side of e-4 results in destruction of the attention layer, and results in just random noise like below:

![Noise](https://user-images.githubusercontent.com/95531133/231112152-a2657014-dfb7-40df-88d3-948efc714ceb.png)

# Additional Information
Learning rate (rho) is set by default at 2e-5, which is apparently good for single subject learning.
I have not been able to reproduce the paper, albeit I have only computed masks with around 50 runs through the dataset (The researchers use 1000). A few images from the training dataset (20 prior images from the class dog, and 5 images of a dog as the concept) (warning: the prior images are also diffusion-generated) are below:
## Class Images:
![4 (1)](https://user-images.githubusercontent.com/95531133/231116615-4f2750b2-3b8f-4e49-b115-03882af1b3a0.jpg)
![0 (1)](https://user-images.githubusercontent.com/95531133/231116921-40212af1-164b-402a-8c91-dbee8aedc084.jpg)
![7](https://user-images.githubusercontent.com/95531133/231116948-bac2e8be-96db-417f-b9ac-a43dbedf532a.jpg)

## Concept:
![0](https://user-images.githubusercontent.com/95531133/231117099-63a2113d-8889-4871-b743-d95d80f6a97a.jpg)
![2](https://user-images.githubusercontent.com/95531133/231117121-36fc1854-3d7a-4e28-b496-c5a19b43b4fd.jpg)

Here are a few generated images:
## Generated (30 runs, I think):
![image](https://user-images.githubusercontent.com/95531133/231117401-619a9dba-7da0-4e0a-a2d8-2400a0b95c5a.png)
![image](https://user-images.githubusercontent.com/95531133/231117422-a2f25a3c-051f-4ef7-8ab0-171f919d8018.png)
![image](https://user-images.githubusercontent.com/95531133/231117440-64cef9ab-f519-4eec-b282-8cc59db6badf.png)

Not much of the concept was learnt.
The implementation is slow, it would be nice to have pointers on how to optimize it (This is my first paper implementation).
PRs are welcome!
Currently the method runs on colab GPUs with around 8-9 GB VRAM on fp16.

# To do:
1.Restore default attention weights after each cones_nference
2.Get attention weights directly instead of looping over all Unet modules.


