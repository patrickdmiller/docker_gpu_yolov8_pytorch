builds container for yolov8 with gpu

usually run and mount volumes for /data and /model

```bash
docker run  --ipc=host --gpus all -it --rm -v /app/data/model_training/:/data/ -v /app/model/:/model/ yolov8 
```

quick hits

```bash
#train
cd /model && yolo task=detect mode=train model=yolov8n.pt data=/data/data.yaml epochs=2 imgsz=800 batch=32 workers=20 save=True name=test exist_ok=True

#resume nondestructive
cd /model && yolo task=detect mode=train model=/model/runs/detect/test/weights/last.pt data=/data/data.yaml epochs=70 imgsz=800 batch=32 workers=20 save=True name=test_2 exist_ok

#inference for all images in folder with best
cd /model && yolo task=detect mode=predict model=runs/detect/test_2/weights/best.pt conf=0.20 source=/data/some_images save=True
```

runs on
```bash
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.85.05    Driver Version: 525.85.05    CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  Off |
|  0%   49C    P8    14W / 450W |      1MiB / 24564MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```