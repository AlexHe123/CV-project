# CV-project
## readme

# introduction

本代码用于测试算法的实时性能，测试思路为：

**实时读取一段视频中一帧图像 → 图像预处理 → 算法处理输入图像，估计深度图 → 依据视差图，从输入图像中采样获取估计的右图 → 将得到的右图后处理并显示到显示器上 → 读取并处理下一帧图像……**

基于上述的算法实时性能测试的流程，使用者可以通过观察显示器上画面的流畅程度，来判断算法的实时性能。

关于实时测试更多的设置和结果，请参考10月15日的汇报ppt，该ppt已上传到公共网盘上，文件名为*2d-to-3d-report1015.pptx*

# 环境说明

- 该代码应当在**带有显示器的工作站**上运行，不要在无显示器的服务器上运行。
- 必要的 python 包及版本要求为：
  - python == 3.6
  - PyTorch == 1.6.0
  - opencv == 3.4.2
  - easydict == 1.9

# 如何运行？

在 spyder 下，直接运行 *main.py* 文件即可。

# 参数说明

本节对 *main.py* 文件中的一系列参数作说明：
- 'data_dir': 输入的视频文件路径
- 'model_path': 读取的模型文件路径。我们提供了 resnet 和 shufflenet 两个模型文件。
- 'input_height': 输入视频图像的高度
- 'input_width': 输入视频图像的宽度
- 'model': 读取的模型结构。可选参数为 'resnet18_md' 或 'shufflenet'，该参数需要和 'model_path' 共同修改，正确搭配。
- 'mode': 运行模式。指定为 'test'
- 'device': 设备名称。指定为 'cuda:0'
- 'input_channels': 输入图像的通道数。指定为 3
- 'num_workers': 读取数据时同时启用的线程数。指定为 8
- 'use_multiple_gpu': 是否使用多 GPU。指定为 False

其余参数保持默认即可。

# 计时说明

cuda 会导致 time 包出现计时不同步的问题，为了得到正确的计时结果，需要在每个 *time.time()* 计时语句前，添加一句 cuda 同步语句 *torch.cuda.synchronize()*

在 *main_real_time.py* 文件中，读取视频每一帧图像并处理的代码为第 146 行到第 212 行。在这一段语句中，分段计时了整个处理流程中每个步骤的耗时。
