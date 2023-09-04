# AlphaPose tensorrt 部署demo



测试环境   cuda_11.8  RTX4060   tensorrt8.6.1    python3.8 

```bash
git clone https://github.com/oreo-lp/AlphaPose_TRT

python setup.py build develop
```

## YOLOv3-SPP(PyTorch) to engine

YOLOv3-SPP(PyTorch)转成dynamic shape的engine模型。我们输入的数据尺寸可以是动态变化的，但是变化的范围要在我们转成engine时所设置的范围内。

(1) YOLOv3_SPP模型转成onnx模型-------输入数据的默认尺寸为: -1x3x608x608 (-1表示batch size可变)

```bash
python ./darknet2onnx.py --cfg ./detector/yolo/cfg/yolov3-spp.cfg --weight ./detector/yolo/data/yolov3-spp.weights
```

(2) 对onnx模型进行优化

```bash
polygraphy surgeon sanitize yolov3_spp_static.onnx --fold-constants --output yolov3_spp_static_folded.onnx
```

(3)由onnx模型转成引擎

minShapes设置能够输入数据的最小尺寸，optShapes可以与minShapes保持一致，maxShapes设置输入数据的最大尺寸，这三个是必须要设置的，可以通过trtexec -h查看具体用法。**/home/peter/tensorrt/TensorRT-8.6.1.6.Linux.x86_64-gnu.cuda-11.8/TensorRT-8.6.1.6/bin/trtexec可以根据环境变量自己添加。**

```bash
/home/peter/tensorrt/TensorRT-8.6.1.6.Linux.x86_64-gnu.cuda-11.8/TensorRT-8.6.1.6/bin/trtexec --onnx=yolov3_spp_static_folded.onnx --explicitBatch --saveEngine=yolov3_spp_static_folded.engine --workspace=10240 --fp16 --verbose
```

## FastPose(PyTorch) to engine

(1) FastPose转成onnx模型

模型输入数据的默认尺寸为：-1x3x256x192 (-1表示batch size可变)

```bash
python pytorch2onnx_dynamic.py --cfg ./configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml --checkpoint ./pretrained_models/fast_res50_256x192.pth
```

(2)onnx模型转成引擎模型

```bash
/home/peter/tensorrt/TensorRT-8.6.1.6.Linux.x86_64-gnu.cuda-11.8/TensorRT-8.6.1.6/bin/trtexec --onnx=alphaPose_-1_3_256_192_dynamic.onnx --saveEngine=alphaPose_-1_3_256_192_dynamic.engine --workspace=10240 --fp16 --verbose --minShapes=input:1x3x256x192 --optShapes=input:1x3x256x192 --maxShapes=input:128x3x256x192 --shapes=input:1x3x256x192 --explicitBatch
```















