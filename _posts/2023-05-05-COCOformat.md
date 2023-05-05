---
layout: single
title: "COCO data Format"
category: DLM
tag: [coco, python, DL]
toc: true
toc_sticky: true

---

# COCO data format

```python
from pycocotools.coco import COCO
```

```python
coco = COCO('./coco.json')
```

    loading annotations into memory...
    Done (t=0.05s)
    creating index...
    index created!

## COCO data format을 자유자재로 사용

### getAnnIds

`coco.getAnnIds()`는 annotation id를 return해준다.

```python
coco.getAnnIds()
```

    [1,
     2,
     3,
     4,
     5,
     6,
     7,
     8,
     9,
     ...]

`coco.getAnnIds()`에 imgIds, catIds를 input으로하여 annotation id를 return 한다.

```python
coco.getAnnIds(imgIds=1)
```

    [1, 2, 3, 4, 5]

```python
coco.getAnnIds(imgIds=1, catIds=1)
```

    [1]

```python
coco.getAnnIds(catIds=1)
```

    [1,
     6,
     11,
     16,
     21,
     27,
     31,
     37,
     41,
     47,
     51,
     ...]

### loadAnns

`coco.loadAnns()`는 annotation id를 input으로 하여 annotation dict를 return한다.

```python
coco.loadAnns(1)
```

    [{'id': 1,
      'category_id': 1,
      'bbox': [226, 184, 164, 28],
      'image_id': 1,
      'segmentation': [[226, 184, 389, 184, 390, 212, 229, 212]],
      'area': 4592,
      'iscrowd': 0}]

### getCatIds

`coco.getCatIds()`는 category id를 return 해준다.

```python
coco.getCatIds()
```

    [1, 2, 3, 4]

### loadCats

`coco.loadCats()`는 category id를 input으로 하여 category dict을 return한다.

```python
print(coco.loadCats(1))
coco.loadCats(coco.getCatIds())
```

    [{'id': 1, 'name': 'eyes', 'supercategory': 'label'}]
    
    
    
    
    
    [{'id': 1, 'name': 'eyes', 'supercategory': 'label'},
     {'id': 2, 'name': 'face', 'supercategory': 'label'},
     {'id': 3, 'name': 'cheek', 'supercategory': 'label'},
     {'id': 4, 'name': 'nose', 'supercategory': 'label'}]

### getImgIds

`coco.getImgIds()`는 img id, category id를 input으로 하여 image id를 return 한다.

```python
coco.getImgIds()
```

    [1,
     2,
     3,
     4,
     5,
     6,
     7,
     8,
     9,
     10,
     ...]

```python
coco.getImgIds(catIds=1)
```

    [1,
     2,
     3,
     4,
     5,
     6,
     7,
     8,
     9,
     10,
     ...]

### loadImgs

`coco.loadImgs()`는 image id를 input으로 하여 annotation의 image dict을 return한다.

```python
coco.loadImgs(1)
```

    [{'id': 1,
      'file_name': 'FLIR3318.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480}]

```python
coco.loadImgs(coco.getImgIds())
```

    [{'id': 1,
      'file_name': 'FLIR3318.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 2,
      'file_name': 'FLIR0485.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 3,
      'file_name': 'FLIR0096.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 4,
      'file_name': 'FLIR0570.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 5,
      'file_name': 'FLIR0838.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     ...]

## Tips

```python
# getImgIds를 이용해 image id를 받고, loadImgs를 이용해 image의 정보를 list로 return.
coco.loadImgs(coco.getImgIds()) 
```

    [{'id': 1,
      'file_name': 'FLIR3318.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 2,
      'file_name': 'FLIR0485.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 3,
      'file_name': 'FLIR0096.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 4,
      'file_name': 'FLIR0570.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 5,
      'file_name': 'FLIR0838.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
     {'id': 6,
      'file_name': 'FLIR4574.jpg',
      'RESOLUTION': 307200,
      'width': 640,
      'height': 480},
      ...]

```python
img_name_to_id = {image_info['file_name']: image_info['id'] for image_info in coco.loadImgs(coco.getImgIds())}
img_name_to_id
```

    {'FLIR3318.jpg': 1,
     'FLIR0485.jpg': 2,
     'FLIR0096.jpg': 3,
     'FLIR0570.jpg': 4,
     'FLIR0838.jpg': 5,
     'FLIR4574.jpg': 6,
     'FLIR1944.jpg': 7,
     'FLIR4206.jpg': 8,
     'FLIR3459.jpg': 9,
     'FLIR4250.jpg': 10,
      ...}

> filename과 image id를 match

```python
 # getCatIds 이용해 category id를 받고, loadCats 이용해 category 정보를 list로 return.
coco.loadCats(coco.getCatIds())
```

    [{'id': 1, 'name': 'eyes', 'supercategory': 'label'},
     {'id': 2, 'name': 'face', 'supercategory': 'label'},
     {'id': 3, 'name': 'cheek', 'supercategory': 'label'},
     {'id': 4, 'name': 'nose', 'supercategory': 'label'}]

```python
cat_name_to_id = {cat_info['name']: cat_info['id'] for cat_info in coco.loadCats(coco.getCatIds())}
cat_name_to_id
```

    {'eyes': 1, 'face': 2, 'cheek': 3, 'nose': 4}

> catagory와 category id를 match
