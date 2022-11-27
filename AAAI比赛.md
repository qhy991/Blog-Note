## Track 1
很多网络都只从FLOPs和Memory角度关心理论加速，忽视了在硬件上的实际加速，在实际硬件上获得理论加速有很多挑战。为此比赛要求设计能达到高精度的更简单部署的网络。
参赛者需要选择一个特定的平台（Atlas300/RV1126 for smart city or camera, GPU T4 for cloud computing, mobile GPU/DSP for mobile phones, and TDA4VM for autonomous driving）
依据硬件特性，开发高效强大的网络。
## Track2
由于不同硬件不同的特性，对于针对硬件设计的网络，不能在所有的硬件上都获得同样的加速。找到能够在不同硬件上获得速度和精度兼顾的网络还有很多挑战，为此比赛要求设计能跑在它提供的所有平台上的高效网络。

### 比赛规则
2022.11.20申请
提交开放时间为2022.12.1-12.31，11.20能下载数据集
两张Tesla V100卡用来训练。
推荐使用United Perception框架来训练
提供了一些[例子](https://github.com/ModelTC/AAAI2023_EAMPD)
每天能够测试模型延时3次，每3天会平台会评估模型精度


- [ ] 把se block去掉 
- [ ] 量化 qat和ptq都试一下

repopt

```python
if 'B1' in config.MODEL.ARCH:

	num_blocks = [4, 6, 16, 1]

	width_multiplier = [2, 2, 2, 4]

elif 'B2' in config.MODEL.ARCH:

	num_blocks = [4, 6, 16, 1]

	width_multiplier = [2.5, 2.5, 2.5, 5]

elif 'L1' in config.MODEL.ARCH:

	num_blocks = [8, 14, 24, 1]

	width_multiplier = [2, 2, 2, 4]

elif 'L2' in config.MODEL.ARCH:

	num_blocks = [8, 14, 24, 1]

	width_multiplier = [2.5, 2.5, 2.5, 5]
```

0.1的数据用作test
训练300epoch
### RepVGG
A0
Max accuracy: 88.23%
### RepOpt
B1
Max accuracy: 90.91%

针对A0的训练，需要搜索一个scale，执行如下代码：
```sh
python3 -m torch.distributed.launch --nproc_per_node 1 --master_port 12349 main_repopt.py --data-path /data/AAAI/Awesome-Backbones/datasets --arch RepOpt-VGG-A0-hs --batch-size 32 --tag search --opts TRAIN.EPOCHS 240 TRAIN.BASE_LR 0.1 TRAIN.WEIGHT_DECAY 4e-5 TRAIN.WARMUP_EPOCHS 10 MODEL.LABEL_SMOOTHING 0.1 DATA.DATASET imagenet 
```

