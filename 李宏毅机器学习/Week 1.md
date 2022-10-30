# 机器学习就是帮人类找一个自己写不出来的复杂函数

[[2022-07-21]]

## 类神经网络：
输入：向量、矩阵、序列
输出：数值（regression）、类别（classification）、文字、图片


## 学习方法：
* **Supervised learning** ------（lecture 1-lectute 6）
	* Training Data
	* label
* **Self-supervised learning** -----(lecture 7)
	* Pre-trained Model（Foundation Model）
		* BERT
			* 340M parameters
	* Downstream Tasks(下游任务)
* **Generative Adversarial Network** ------ (lecture 6)
* **Reinforcement Learning(RL)** -----(lecture 12)


## Anomaly Detection(异常检测) -----Lecture 8
## Explainable AI(可解释性) -----Lecture 9
## Model Attck(模型攻击) -----Lecture 10
## Domain Adaptation -----Lecture 11
## Network Compression(模型压缩) ------Lecture 13
## Life-long Learning -----Lecture 14
## Meta Learning -----Lecture 15 
Few-shot learning往往需要做到Meta learning


# Pytorch Tutorial
## Training Neural Network
* Define Neural Network
* Loss Fuction
* Optimization Algorithm

## Training & Testing Neural Network ----- in Pytorch
### Step 1: Load Data
* torch.utils.data.Dataset
	* 把原始资料加载进来，打包为类，方便之后调取
* torch.utils.data.Dataloader
	* 把Dataset中的数据进行batchsize的读取

```python
from torch.utils.data import Dataset, Dataloader

class MyDataset(Dataset):
	def __init__(self,file):
		#Read data
		self.data = ...


	def __gititem__(self, idex):
		# Returns one samples at a time
		return self.data[idex]


	def __len__(self):
		# Returns the size of the dataset
		return len(self.data)


dataset = Mydataset(file)
dataloader = Dataloader(dataset, batch_size = 5, shuffle = false)

```

### Tensor
[Welcome to PyTorch Tutorials — PyTorch Tutorials 1.12.0+cu102 documentation](https://pytorch.org/tutorials/)
High-dimensional matrices
* 可以用GPU做加速
	* torch.cuda.is_available()
* 梯度运算方便
	* x = torch.tensor()
	* z = 2*x
	* z.backword()
	* x.grad

### Step 2: Define Neural Network
* torch.nn Module
```python
class MyModel(nn.Moudle):
	super(MyModel, self)
```


### Step 3: Loss Fuction


### Step 4: Optimization algorithms
