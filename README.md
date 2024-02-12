# LSUN-Bedroom-Latent-Diffusion-UNet-with-Layer-Architectures-and-Hyperparameters-Experiment  
My personal practice, try to train LSUN Bedroom Diffusion Unet (from Diffusers), from sratch, with different Layer Architectures and Hyperparameters.  

# Main Objective  
Try out different Layer Architectures and Hyperparameters for training Diffusers UNet on LSUN Bedroom dataset.  

# What to expect  
1. Layer Architecture experiment overall insights.  
2. Hyperparameters experiment overall insights.

# Prequisite  
1. Know how deep learning works in general.
2. Know Latent Diffusion pipeline.
3. Know Pytorch.
4. Know Fastai.
5. Know Activation Stats (Fastai). 

# Files    
1. Original files from Fastai Course Part 2, Lesson 25 (from [Course link](https://course.fast.ai/)):  
- *30_lsun_diffusion-latents.ipynb*
2. Origianl experiment file:
- *image_generator_nonorm.ipynb*  
3. Other experiment files:
- Other files, I alternate Layer Architectures and Hyperparameters from the Original experiment file.
(Mostly in Hyperparameter section, Latents Dataset/DataLoader section and Training/Init Unet section in notebooks)

# File's name meaning:  
The notebooks' name and files's name in result folder generally indicate the experiment I did.  
(Details can be checked in notebooks)  
(Mostly in Hyperparameter section, Latents Dataset/DataLoader section and Training/Init Unet section in notebooks)  

E.g: *image_generator_(32,32,64,64,128)_(DAAAA)_InitXavierNormRelu_Conv_BN_Bs16_Lr2e-4_10k_60e.ipynb*  
- *(32,32,64,64,128)*: block_out_channels  
- *(DAAAA)*: down_block_types  
- *InitXavierNormRelu*: Xavier Normalization is used  
- *Conv*: Origianl model's LoRACompatibleConv layers are replaced with Conv2d layers  
- *BN*: Origianl model's Group Norm layeres in ResnetBlocks are replaced with Batch Norm layers  
- *Bs16*: batch size 16  
- *Lr2e-4*: learning rate 2e-4  
- *10k*: Input dataset size 10k (subset of dataset)  
- *60e*: 60 epochs  

Notice:
- Files with or without *nonorm* in the name, are done on **unormalized dataset**.
- Subset 10k dataset is saved in data folder. For other subset, you have to wait for the **"lengthy"** latents process.
(subset 10k dataset takes about 1 hour to be encoded into latent space) 

# Result  
Although, there are quite some experiments I try out, they are not really arranged well, and also some missing notebooks (I alternated hyperparameters little by little and forgot to save each individual changes). Therefore, it can be a pain to go through looking at the results to find meaningful patterns.  

For that, I will try to show the insights of which Layer Atchitecture and Hyperparameters changing has significant impact to the training model process result.  

# Insight  
1. With the Original file from Fastai Course Part 2, Lesson 25, training process goes on smoothly. However, when I use Diffusers UNet with the same setups, the training process does not go well, the loss rocketed after training for a while.  
2. Because some of my experiments involve changing Noisify function, which affect the Loss, meaning smaller Loss does not always mean better training. Therefore, I use Activation Stats to examine models training performance.

Activation Stats has 3 main graphs:  
- **Mean/Standard deviation (Mean/Std) graph**: show mean and std of activation layers.  
- **Color Dimension (Color Dim)**: show histogram values of activation layers.  
- **Dead Chart**: show volume of value equal or near zero of activation layers.  

- For **Mean/Std graph**, we want the Mean to be around 0 and Std to be around 1 at the start. We also want the value to be consistent, through out the whole training process.  
- For **Color Dim**, we want the value to equally spread out, through out the whole training process.  
- For **Dead Chart**, we want the value equal or near zero to be really low, throgh out the whole training process.  

3. The Original file's Activation Stats (rebuilt version) (Good): *image_generator_nonorm_miniai_100k_20e.ipynb*

<img scr="images/image_generator_nonorm_miniai_100k_20e.JPG" width="200" height="400">

5. One of the bad result experiment (Bad): (I did not save to source file, but I saved the result)

<img scr="images/stats_(32,64,64,128)_(DAAD)_InitXavierNormRelu_BatchNormRN_60e.JPG" width="200" height="400">

6. One of the good result experiment (Good): *image_generator_(32,32,64,64,128,128)_(DDAADD)_InitXavierNormRelu_Lr1e-5_100k_25e.ipynb*

<img scr="images/image_generator_(32,32,64,64,128,128)_(DDAADD)_InitXavierNormRelu_Lr1e-5_100k_25e.JPG" width="200" height="400">

7. Generally, typical Mean/Std good graphs and bad graphs will look something like this below:

Good  
<img scr="images/good_mean_std_graph.JPG" width="200" height="200">  

Bad  
<img scr="images/bad_mean_std_graphh.JPG" width="200" height="200">  

**(x axis is the number of batches)**  

8. For this Latent Diffusion UNet with LSUN Bedroom dataset, here are Layer Architectures and Hyperparameters patterns I tried that gave the good Mean/Std graphs, or something similar: 
- Try Batch Norm layers.  
- Try smaller Learning Rate.  
- Try smaller Batch.  
- For Diffusers UNet, try more blocks.
- For Diffusers UNet, try more block_out_channels.
- For Diffusers UNet, try more Attention blocks. 
