---
title: Install Anaconda On the Linux Server
date: 2024-02-06 22:48:04
toc: true
tags:
    - Project Tools
categories: blog
excerpt: This is a brief introduction about how to install the anaconda on the linux server.
---

## Download the Anaconda Package
Firstly, we need to get the anaconda3 package and there is some mirrors website providing the faster speed of downloading. There, we chosen the tsinghua mirror and the version of 2023.09 with x86 architecture. 
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2023.09-0-MacOSX-x86_64.sh
```
After running, we can get this package at the home directory.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.5r5389axslo0.webp" alt="downloaded file" width="70%"/>

## Install the Package
In the linux server, we can install the anaconda package by the following command.
```bash
bash Anaconda3-2023.09-0-Linux-x86_64.sh
```
Running the above command we can see this welcoming page, 
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.4i9vwhn97wk0.webp" alt="welcoming page" width="70%"/>

Next, we need to accept the license agreement to continue following steps. 
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.4rl5mbp8bts0.webp" alt="license agreement" width="70%"/>

Then, we need to choose the installation path, and we can choose the default path by inputting ENTER.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.2mz6gg3g1eq0.webp" alt="configure installation path" width="70%"/>

Automatically initializing conda will be convenient for us. So in this step, it is recommended to input the yes for automatically activating base environment.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.48pgizhm7es0.webp" alt="automatically initialize conda" width="70%"/>

After implementing the above steps, we can successfully install the anaconda on a linux server.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.2jq7cjfsovq0.webp" alt="successful page" width="70%"/>

There is two ways to check whether the anaconda is installed successfully. The first one is to check the version of anaconda. The second one is to check the environment variable of anaconda.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.101dmkqh5h8w.webp" alt="check installation" width="70%"/>

## Create the New Environment
In this section, a new conda virtual environment will be create:
```conda
conda create -n LabelNoise python=3.11.5
```
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.1k1n4ytgcx40.webp" alt="create a virtual environment" width="70%"/>

We can also specify the installation path of the new environment. For example, we can create a new environment in the path of `/home/zhli/anaconda3/envs/Pytorch` by the following command:
```conda
conda create --prefix=/home/zhli/anaconda3/envs/Pytorch python=3.11.5
```
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.7c6r4gjz3pk0.webp" alt="specify installation path" width="70%"/>

## Activate the New Environment
If we want use the new environment, we need to activate it first.
```conda
conda activate LabelNoise
```
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.55px3u0fi2w0.webp" alt="activate virtual environment" width="70%"/>

## Remove the New Environment
If we donn't need the new environment any more, we can remove it by the following command:
```conda
conda remove --n Pytorch --all
```
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.6a95me4l7gw0.webp" alt="remove virtual environment" width="70%"/>