---
title: 基于YOLO模型的目标检测与识别实现在ESP32-S3上全流程部署
date: 2024-11-01 11:10:39
toc: true
tags:
    - Deep Model Deployment
    - Object Detection
categories: projects
excerpt: 文章主要基于YOLO模型在自定义的数据集上训练，实现特定类别的目标检测与识别，并将训练好的模型部署在ESP32-S3上。
---
## 项目环境安装
### ESP-IDF安装
ESP-IDF 5.0+ 的版本有较大改动，在部署过程中会出现一些问题，建议使用 4.4 版本的进行安装。
基于 Windows 平台的软件安装，可以参考 [https://dl.espressif.com/dl/esp-idf/](https://dl.espressif.com/dl/esp-idf/). 按照流程完成安装即可。

### 开发环境
本项目整体开发环境主要基于训练框架，以及对应esp32的模型部署框架，具体如下：
1. 训练、转换模型: [Model Assistant](https://github.com/Seeed-Studio/ModelAssistant)
2. 模型部署: [sscma-example-esp32(1.0.0)](https://github.com/Seeed-Studio/sscma-example-esp32/tree/1.0.0)

### 运行环境
```python
python3.10 + CUDA11.7 + esp-idf 4.4
# 主要按照 ModelAssistant/requirements_cuda.txt 进行安装
torch                        2.0.0+cu117
torchaudio                   2.0.1+cu117
torchvision                  0.15.1+cu117
yapf                         0.40.2
typing_extensions            4.5.0
tensorboard                  2.13.0
tensorboard-data-server      0.7.2
tensorflow                   2.13.0
keras                        2.13.1
tensorflow-estimator         2.13.0
tensorflow-intel             2.13.0
tensorflow-io-gcs-filesystem 0.31.0
sscma                        2.0.0rc3
setuptools                   60.2.0
rich                         13.4.2
Pillow                       9.4.0
mmcls                        1.0.0rc6
mmcv                         2.0.0
mmdet                        3.0.0
mmengine                     0.10.1
mmpose                       1.2.0
mmyolo                       0.5.0
```

conda 的环境依赖主要见上面各种库的版本，其中 

+ `mmcv` 库安装
`mmcv` 库的安装需要对应 `cuda` 版本、`torch` 版本以及 `python` 版本，具体说明：cu117，torch2.0.0，python3.10可以参考
[https://download.openmmlab.com/mmcv/dist/cu117/torch2.0.0/index.html](https://download.openmmlab.com/mmcv/dist/cu117/torch2.0.0/index.html)，对应其中主要根据操作系统选择性安装，`./mmcv-2.0.1-cp310-cp310-manylinux1_x86_64.whl` 和 `./mmcv-2.0.1-cp310-cp310-win_amd64.whl` 文件，具体如下，
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-11-01%20114502.png" alt="mmcv库安装" width="70%"/>

## 训练数据集准备
### 开源 VOC 数据集
从官网下载 [VOC2007数据集](http://host.robots.ox.ac.uk/pascal/VOC/voc2007/index.html)，并解压到指定目录 `data`中，利用下面代码将 VOC 格式的标注文件转换为对应 COCO 格式的 json 文件。
{% message color:info %}
训练选取的类别主要有，`["person", "cellphone", "cup", "table", "chair"]`，其中对于 `person` 类别主要采用 VOC2007数据集训练，而 `"cellphone", "cup", "table", "chair"` 类别均额外收集了对应的数据集进行汇总，具体汇总方法见下文。
{% endmessage %}
```python
import os
import json
import pandas as pd
from xml.etree import ElementTree as ET
from PIL import Image
import shutil
import random
from tqdm import tqdm

# Set paths
voc_path = 'data/VOCdevkit/VOC2007'
train_path = 'datasets/collection/train'
valid_path = 'datasets/collection/valid'

# Create directories if not exist
if not os.path.exists(train_path):
    os.makedirs(train_path)
if not os.path.exists(valid_path):
    os.makedirs(valid_path)

# training classes
classes = ["person", "cellphone", "cup", "table", "chair"]

# Get list of image files
image_files = os.listdir(os.path.join(voc_path, 'JPEGImages'))
random.seed(0)
random.shuffle(image_files)

# Split data into train and valid
train_files = image_files[:int(len(image_files)*0.8)]
valid_files = image_files[int(len(image_files)*0.8):]

# Convert train data to COCO format
train_data = {'categories': [], 'images': [], 'annotations': []}
train_ann_id = 0
train_cat_id = 0
img_id = 0
train_categories = {}
for file in tqdm(train_files):
    # Add annotations
    xml_file = os.path.join(voc_path, 'Annotations', file[:-4] + '.xml')
    tree = ET.parse(xml_file)
    root = tree.getroot()
    for obj in root.findall('object'):
        category = obj.find('name').text
        if category not in classes:
            continue
        if category not in train_categories:
            train_categories[category] = train_cat_id
            train_cat_id += 1
        category_id = train_categories[category]
        bbox = obj.find('bndbox')
        x1 = int(bbox.find('xmin').text)
        y1 = int(bbox.find('ymin').text)
        x2 = int(bbox.find('xmax').text)
        y2 = int(bbox.find('ymax').text)
        width = x2 - x1
        height = y2 - y1
        ann_info = {'id': train_ann_id, 'image_id': img_id, 'category_id': category_id, 'bbox': [x1, y1, width, height],
                   'area': width*height, 'iscrowd': 0}
        train_data['annotations'].append(ann_info)
        train_ann_id += 1
        
    if len(root.findall('object')):
        image_id = img_id
        img_id += 1
        image_file = os.path.join(voc_path, 'JPEGImages', file)
        shutil.copy(image_file, os.path.join(train_path, file))
        img = Image.open(image_file)
        image_info = {'id': image_id, 'file_name': file, 'width': img.size[0], 'height': img.size[1]}
        train_data['images'].append(image_info)

# Add categories
for category, category_id in train_categories.items():
    train_data['categories'].append({'id': category_id, 'name': category})
```
### 添加自定义数据集
主要的数据集可以从开源的 [Roboflow Universe](https://universe.roboflow.com/) 搜集，比如我们需要识别某些类别，可以在该网站上下载对应的数据集，下载格式选择 COCO 格式，如下图所示：
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241101120252.png" alt="数据集下载示例" width="70%"/>

挑选多个类别数据集后，需要对其进行合并，在上面 VOC2007 数据集处理的基础上，利用下述代码进行合并（**主要合并 json 文件，各类别数据集的图像文件直接移动即可**）：
```python
# Add class data
def add_coco_class_data(dataset_path, classname, img_id, train_ann_id, category_id):
    with open(os.path.join(dataset_path, '_annotations.coco.json'), 'r') as file:
        class_data = json.load(file)
    class_exist, class_id = False, None
    for item in train_data['categories']:
        if item['name'] == classname:
            class_exist, class_id = True, item['id']
    
    if class_exist is False:
        category_id += 1
        class_id = category_id
        train_data['categories'].append({'id': category_id, 'name': classname})
    
    print(train_data['categories'])

    # add the class images for the train data
    image_id = img_id
    for image in class_data['images']:
        image_info = {'id': img_id, 'file_name': image['file_name'], 'width': image['width'], 'height': image['height']}
        train_data['images'].append(image_info)
        img_id += 1
    
    # add the class annotations for the train data
    for ann in class_data['annotations']:
        ann_info = {'id': train_ann_id, 'image_id': image_id+ann['image_id'], 'category_id': class_id, 'bbox': ann['bbox'],
                    'area': ann['area'], 'iscrowd': ann['iscrowd']}
        train_data['annotations'].append(ann_info)
        train_ann_id += 1
    
    return img_id, train_ann_id, category_id

# process the extra dataset
chair_path = './Dataset/Collection/deteksi kursi.v1i.coco/train'
phone_path = './Dataset/Collection/cellphone.v1i.coco/train'
cup_path = './Dataset/Collection/cups detection.v1i.coco/train'
table_path_1 = './Dataset/Collection/Detect Tables.v1i.coco/train'
table_path_2 = './Dataset/Collection/T4bl3.v1i.coco/train'
img_id, train_ann_id, category_id = add_coco_class_data(chair_path, "chair", img_id, train_ann_id, category_id)
img_id, train_ann_id, category_id = add_coco_class_data(phone_path, "cellphone", img_id, train_ann_id, category_id)
img_id, train_ann_id, category_id = add_coco_class_data(cup_path, "cup", img_id, train_ann_id, category_id)
img_id, train_ann_id, category_id = add_coco_class_data(table_path_1, "table", img_id, train_ann_id, category_id)
img_id, train_ann_id, category_id = add_coco_class_data(table_path_2, "table", img_id, train_ann_id, category_id)
```
随后保存训练数据文件，
```python
# Save train data to file
with open(os.path.join(train_path, '_annotations.coco.json'), 'w') as f:
    json.dump(train_data, f, indent=4)
```
对于测试数据集，处理方式与训练数据集类似，汇总如下：
```python
# Convert valid data to COCO format
valid_data = {'categories': [], 'images': [], 'annotations': []}
valid_ann_id = 0
img_id = 0
for file in tqdm(valid_files):
    # Add annotations
    xml_file = os.path.join(voc_path, 'Annotations', file[:-4] + '.xml')
    tree = ET.parse(xml_file)
    root = tree.getroot()
    for obj in root.findall('object'):
        category = obj.find('name').text
        if category not in classes:
            continue
        category_id = train_categories[category]
        bbox = obj.find('bndbox')
        x1 = int(bbox.find('xmin').text)
        y1 = int(bbox.find('ymin').text)
        x2 = int(bbox.find('xmax').text)
        y2 = int(bbox.find('ymax').text)
        width = x2 - x1
        height = y2 - y1
        ann_info = {'id': valid_ann_id, 'image_id': img_id, 'category_id': category_id, 'bbox': [x1, y1, width, height],
                   'area': width*height, 'iscrowd': 0}
        valid_data['annotations'].append(ann_info)
        valid_ann_id += 1
        
    if len(root.findall('object')):
        # Add image
        image_id = img_id
        img_id += 1
        image_file = os.path.join(voc_path, 'JPEGImages', file)
        shutil.copy(image_file, os.path.join(valid_path, file))
        img = Image.open(image_file)
        image_info = {'id': image_id, 'file_name': file, 'width': img.size[0], 'height': img.size[1]}
        valid_data['images'].append(image_info)

# Add categories
valid_data['categories'] = train_data['categories']

def add_coco_class_data_val(dataset_path, classname, img_id, train_ann_id, category_id):
    with open(os.path.join(dataset_path, '_annotations.coco.json'), 'r') as file:
        class_data = json.load(file)
    class_exist, class_id = False, None
    for item in valid_data['categories']:
        if item['name'] == classname:
            class_exist, class_id = True, item['id']
    
    if class_exist is False:
        category_id += 1
        class_id = category_id
        valid_data['categories'].append({'id': category_id, 'name': classname})
    
    print(valid_data['categories'])

    # add the class images for the train data
    image_id = img_id
    for image in class_data['images']:
        image_info = {'id': img_id, 'file_name': image['file_name'], 'width': image['width'], 'height': image['height']}
        valid_data['images'].append(image_info)
        img_id += 1
    
    # add the class annotations for the train data
    for ann in class_data['annotations']:
        ann_info = {'id': train_ann_id, 'image_id': image_id+ann['image_id'], 'category_id': class_id, 'bbox': ann['bbox'],
                    'area': ann['area'], 'iscrowd': ann['iscrowd']}
        valid_data['annotations'].append(ann_info)
        train_ann_id += 1
    
    return img_id, train_ann_id, category_id

# process the extra dataset
chair_path = './Dataset/Collection/deteksi kursi.v1i.coco/valid'
phone_path = './Dataset/Collection/cellphone.v1i.coco/valid'
cup_path = './Dataset/Collection/cups detection.v1i.coco/valid'
table_path_1 = './Dataset/Collection/Detect Tables.v1i.coco/valid'
table_path_2 = './Dataset/Collection/T4bl3.v1i.coco/valid'
img_id, valid_ann_id, category_id = add_coco_class_data_val(chair_path, "chair", img_id, valid_ann_id, category_id)
img_id, valid_ann_id, category_id = add_coco_class_data_val(phone_path, "cellphone", img_id, valid_ann_id, category_id)
img_id, valid_ann_id, category_id = add_coco_class_data_val(cup_path, "cup", img_id, valid_ann_id, category_id)
img_id, valid_ann_id, category_id = add_coco_class_data_val(table_path_1, "table", img_id, valid_ann_id, category_id)
img_id, valid_ann_id, category_id = add_coco_class_data_val(table_path_2, "table", img_id, valid_ann_id, category_id)

# Save valid data to file
with open(os.path.join(valid_path, '_annotations.coco.json'), 'w') as f:
    json.dump(valid_data, f, indent=4)
```

## 下载预训练模型
参考 [Face Detection - Swift-YOLO](https://github.com/Seeed-Studio/sscma-model-zoo/blob/main/notebooks/zh_CN/Face_Detection_Swift-YOLO_96.ipynb) 下载预训练模型权重文件 [pretrain.pth](https://files.seeedstudio.com/sscma/model_zoo/detection/face_detection/swift_yolo_1xb16_300e_coco_300_sha1_fe1d7dec30d62e583a7ccf717fd6585c792570bf.pth)，然后保存在 `ModelAssistant/checkpoints` 文件夹下。

## 训练 YOLO 模型
在 ModelAssistant 项目下，采用 yolov5_tiny 的配置文件进行训练，训练命令如下：
```bash
# training the yolo model
python tools/train.py configs/swift_yolo/swift_yolo_tiny_1xb16_300e_coco.py \
--cfg-options \
    work_dir=work_dirs/collection_tiny_ep300 \
    num_classes=5 \
    epochs=300 \
    height=96 \
    width=96 \
    data_root=datasets/collection/ \
    load_from=checkpoints/pretrain.pth
```

## 导出模型
训练完毕后，采用以下命令导出模型：
```bash
# export the model
python tools/export.py configs/swift_yolo/swift_yolo_tiny_1xb16_300e_coco.py ./work_dirs/collection_tiny_ep300/best_coco_bbox_mAP_epoch_300.pth --cfg-options  \
    work_dir=work_dirs/collection_tiny_ep300 \
    num_classes=5 \
    epochs=300  \
    height=96 \
    width=96 \
    data_root=datasets/collection/ \
    load_from=checkpoints/pretrain.pth
```
导出的模型会保存在 `work_dirs/collection_tiny_ep300` 文件夹下，生成 `best_coco_bbox_mAP_epoch_300_int8.tflite` 文件，这是量化到 Int8 格式的tflite 文件，可以用于后续模型的部署。

## References

+ [esp32-s3训练自己的数据进行目标检测、图像分类](https://blog.csdn.net/weixin_45977690/article/details/135137552)
