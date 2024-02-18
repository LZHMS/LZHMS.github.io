---
title: A Template of Daily Log For Recording Your Project
date: 2023-11-02 21:17:31
toc: true
tags:
    - Project Habits
categories: blog
excerpt: I'd like to share my template of daily log to record the project with you.
---
## Daily Log 1

+ Author: Zhihao Li
+ Date: November 2, 2023
+ Project: Robotics

### Stage I: Prepare the environment

### Records And Backups

+ Configure Model Virtual Environment

```python
pip install barbar
pip install imgaug
# based on CPU device
pip3 install torch torchvision torchaudio
```

### Problems And Solutions

#### Problem

`Failed to build mpi4py ERROR: Could not build wheels for mpi4py, which is required to install pyproject.toml-based projects`

#### Solution

`conda install mpi4py`
When I used `pip` to install the `mpi4py`, I encounted the above error. But I changed to `conda` and it workd very well.

#### Problem

`OSError: /lib64/libc.so.6: version GLIBC_2.18' not found when trying to import open3d`

#### Solution

`pip3 install open3d`

### Learning And Summary

+ Learning

  + Data augmentation
    Data augmentation is a common data preprocessing technique used to expand limited datasets and improve the generalization ability of machine learning models. Data augmentation involves applying a series of transformations to the original data, generating new data samples, thereby increasing the dataset's size, diversity, and complexity.
    Data augmentation techniques can be applied to various machine learning tasks such as image classification, object detection, natural language processing, and more. Common data augmentation techniques include:
  + Random cropping: Randomly cropping sub-images of different sizes from the original image.
  + Random flipping: Randomly flipping the image horizontally or vertically.
  + Random rotation: Randomly rotating the image by a certain angle.
  + Random scaling: Randomly changing the size of the image.
  + Noise injection: Adding random noise to the image.
  + Color jittering: Randomly adjusting attributes such as brightness, contrast, and saturation.
+ Summary

  + Build Project Structure

  ```python
  # Tiling.py
  Data{
      Slides{
          ACC_Cases{
              Slide_1, 
              Slide_2,
              ...
          }, 
          ACC Cases Tiles Pass{}, 
          ACC Cases Tiles Fail{},
          ACC_Norm_Cases{
              Slide_1, 
              Slide_2, 
              ...
          }, 
          ACC Norm Cases Tiles Pass{}, 
          ACC Norm Cases Tiles Fail{},
          save_path{
              blured.json
          }
      }
  }
  """Note: Cases file stores all slides; Cases Tiles Pass file stores normal tiles of all sildes, Cases Tiles Fail file stores noise tiles of all sildes."""
  ```
