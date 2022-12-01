# Yolov5 + StrongSORT with OSNet


![result_1](assets/demo_r.gif)

<a href="https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch/actions"><img src="https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch/workflows/CI%20CPU%20testing/badge.svg" alt="CI CPU testing"></a>
<br>  
<a href="https://colab.research.google.com/drive/18nIqkBr68TkK8dHdarxTco6svHUJGggY?usp=sharing"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>
  
</div>

</div>


## Introduction

This repository contains a highly configurable two-stage-tracker that adjusts to different deployment scenarios. The detections generated by [YOLOv5](https://github.com/ultralytics/yolov5), a family of object detection architectures and models pretrained on the [COCO](https://arxiv.org/abs/1405.0312) dataset, are passed to the tracker of your choice. Supported ones at the moment are: [StrongSORT](https://github.com/dyhBUPT/StrongSORT)[](https://arxiv.org/abs/2202.13514) [OSNet](https://github.com/KaiyangZhou/deep-person-reid)[](https://arxiv.org/abs/1905.00953), [OCSORT](https://github.com/noahcao/OC_SORT)[](https://arxiv.org/abs/2203.14360) and [ByteTrack](https://github.com/ifzhang/ByteTrack)[](https://arxiv.org/abs/2110.06864). They can track any object that your Yolov5 model was trained to detect.

## Installation

```
git clone --recurse-submodules https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet.git  # clone recursively
cd Yolov5_StrongSORT_OSNet
pip install -r requirements.txt  # install dependencies
```

<details>
<summary>Tutorials</summary>

* [Yolov5 training (link to external repository)](https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data)&nbsp;
* [Deep appearance descriptor training (link to external repository)](https://kaiyangzhou.github.io/deep-person-reid/user_guide.html)&nbsp;
* [ReID model export to ONNX, OpenVINO, TensorRT and TorchScript](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/ReID-multi-framework-model-export)&nbsp;
* [Evaluation on custom tracking dataset](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/How-to-evaluate-on-custom-tracking-dataset)&nbsp;
* Inference acceleration with Nebullvm
  * [Yolov5](https://colab.research.google.com/drive/1J6dl90-zOjNNtcwhw7Yuuxqg5oWp_YJa?usp=sharing)&nbsp;
  * [ReID](https://colab.research.google.com/drive/1APUZ1ijCiQFBR9xD0gUvFUOC8yOJIvHm?usp=sharing)&nbsp;
  
  </details>
  
<details>
<summary>Experiments</summary>

* [Yolov5 StrongSORT OSNet vs other trackers MOT17](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/MOT-17-evaluation-(private-detector))&nbsp;
* [Yolov5 StrongSORT OSNet vs other trackers MOT16 (deprecated)](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/MOT-16-evaluation)&nbsp;
* [StrongSORT MOT16 ablation study](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/Yolov5DeepSORTwithOSNet-vs-Yolov5StrongSORTwithOSNet-ablation-study-on-MOT16)&nbsp;
* [Effect of different OSNet architectures on MOT16](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/OSNet-architecture-performances-on-MOT16)
* [Yolov5 StrongSORT vs BoTSORT vs OCSORT](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/StrongSORT-vs-BoTSORT-vs-OCSORT)
    * Yolov5 [BoTSORT](https://arxiv.org/abs/2206.14651) branch: https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/tree/botsort
* [Effect of full images vs 1280 input to StrongSORT on MOT17](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/Effect-of-passing-full-image-input-vs-1280-re-scaled-to-StrongSORT-on-MOT17)
* [Effect of KF updates ahead for tracks with no associations on MOT17](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/wiki/Effect-of-KF-updates-ahead-for-tracks-with-no-associations,-on-MOT17)

  </details>

## Tracking

```bash
$ python track.py
```

<details>
<summary>Tracking methods</summary>

```bash
$ python track.py --tracking-method strongsort
                                    ocsort
                                    bytetrack
```
  
</details>

<details>
<summary>Tracking sources</summary>

Tracking can be run on most video formats

```bash
$ python track.py --source 0  # webcam
                           img.jpg  # image
                           vid.mp4  # video
                           path/  # directory
                           path/*.jpg  # glob
                           'https://youtu.be/Zgi9g1ksQHc'  # YouTube
                           'rtsp://example.com/media.mp4'  # RTSP, RTMP, HTTP stream
```

</details>

<details>
<summary>Select Yolov5 model</summary>

There is a clear trade-off between model inference speed and overall performance. In order to make it possible to fulfill your inference speed/accuracy needs you can select a Yolov5 family model for automatic download. These model can be further optimized for you needs by the [export.py](https://github.com/ultralytics/yolov5/blob/master/export.py) script

```bash


$ python track.py --source 0 --yolo-weights yolov5n.pt --img 640
                                            yolov5s.tflite
                                            yolov5m.pt
                                            yolov5l.onnx 
                                            yolov5x.pt --img 1280
                                            ...
```
  
</details>

<details>
<summary>Select ReID model</summary>

Some tracking methods combine appearance description and motion in the process of tracking. For those which use appearance, you can choose a ReID model based on your needs from this [ReID model zoo](https://kaiyangzhou.github.io/deep-person-reid/MODEL_ZOO). These model can be further optimized for you needs by the [reid_export.py](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/blob/master/reid_export.py) script

```bash


$ python track.py --source 0 --reid-weights osnet_x0_25_market1501.pt
                                            mobilenetv2_x1_4_msmt17.engine
                                            resnet50_msmt17.onnx
                                            osnet_x1_0_msmt17.pt
                                            ...
```

</details>
  
<details>
<summary>Filter tracked classes</summary>

By default the tracker tracks all MS COCO classes.

If you want to track a subset of the classes that you model predicts, add their corresponding index after the classes flag,

```bash
python track.py --source 0 --yolo-weights yolov5s.pt --classes 16 17  # COCO yolov5 model. Track cats and dogs, only
```

[Here](https://tech.amikelive.com/node-718/what-object-categories-labels-are-in-coco-dataset/) is a list of all the possible objects that a Yolov5 model trained on MS COCO can detect. Notice that the indexing for the classes in this repo starts at zero

</details>

<details>
<summary>MOT compliant results</summary>
  
Can be saved to your experiment folder `runs/track/<yolo_model>_<deep_sort_model>/` by 

```bash
python track.py --source ... --save-txt
```

</details>

## Cite

If you find this project useful in your research, please consider cite:

```latex
@misc{yolov5-strongsort-osnet-2022,
    title={Real-time multi-camera multi-object tracker using YOLOv5 and StrongSORT with OSNet},
    author={Mikel Broström},
    howpublished = {\url{https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet}},
    year={2022}
}
```

## Contact 

For Yolov5 DeepSort OSNet bugs and feature requests please visit [GitHub Issues](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet/issues). 
For business inquiries or professional support requests please send an email to: yolov5.deepsort.pytorch@gmail.com
