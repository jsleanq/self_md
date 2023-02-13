#PyTorch

##输入

```python
inputs = torch.rand(1, 3, 16, 112, 112)#rand(batch_size, in_channels, N, W, H)
```

##[网络结构（torch.nn）](https://pytorch-cn.readthedocs.io/zh/latest/package_references/torch-nn/)

```python
torch.nn.Module #网络基类， 可用树形结构嵌入模型

add_module(name, module) #将 child module 添加到当前 module
forward(* input) # 所有子类需要重写
parameters(memo=None) #返回一个 包含模型所有参数 的迭代器 常与 optimizer 联合使用
nn.Sequential(* args) #一个时序容器 Modules 会以他们传入的顺序被添加到容器中
append()、entend() #类似 list 用法

Conv1/2/3d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
MaxPool1/2/3d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
AvgPool1/2/3d(kernel_size, stride=None, padding=0, ceil_mode=False, count_include_pad=True)
AdaptiveMaxPool1/2/d(output_size, return_indices=False)

ReLU(inplace=False)
BatchNorm1/2/3d(num_features, eps=1e-05, momentum=0.1, affine=True) 
Linear(in_features, out_features, bias=True)
Dropout(p=0.5, inplace=False)
Dropout2/3d(p=0.5, inplace=False)
```

## 误差函数

```python
criterion = nn.CrossEntropyLoss() #交叉熵
# nn.MSECriterion()最小均方误差
```

## 更新参数

```python
import torch.optim as optim
optimizer = optim.SGD(net.parameters(), lr=0.01, momentum=0.9, weight_decay=5e-4)
optimizer = optim.Adam(net.parameters(), lr=0.01)
```

## 数据读入（图像）

```python
import torchvision #包含一些常见数据集的加载模块，Imagenet，CIFAR10，MNIST

from torch.utils.data import Dataset #利用 torch 导入，主要作用：组合数据集和采样器
#自定义时重写：
# __len__ ：实现 len(dataset) ，返还数据集的尺寸。 
# __getitem__ ： 用来获取一些索引数据，例如 dataset[i] 中的(i)。


from torch.utils.data import DataLoader #利用 torch 导入，主要作用：数据通道
dataloader = DataLoader(dataset,batch_size=5,shuffle=True,num_workers=2) 
#num_work：导入数据的子进程个数。设置为0，就是使用主进程来导入数据。
```

## GPU训练

```python
# CUDA 是一个新的基础架构， 适用于 GPU ，硬件的直接访问接口
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu") #选择 CUDA

#并行训练需要把模型和数据都 load 进 GPU 中
device_ids = [0, 1]
model = torch.nn.DataParallel(model.cuda(), device_ids=device_ids) #在两块卡上并行训练模型，默认在0卡上合并
```

## 保存和加载模型

```python
torch.save # 将序列化对象保存到磁盘。此函数使用 Python 的 pickle 模块进行序列化。使用此函数可以保存如模型、tensor、字典等各种对象。
torch.save(model.state_dict(), PATH)

torch.load # 使用 pickle 的 unpickling 功能将 pickle 对象文件反序列化到内存。此功能还可以有助于设备加载数据。
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.eval()
```

