# Pytorch经验总结

## 梯度与反向传递

- 基于链式法则构建计算图，当`.requires_grad`为`True`时对张量的操作会被记录进入计算图

- 不要进行张量对张量的求导，如果张量调用`.backward()`需要传入同形权重张量$w$进行**加权求和**

- 不想被追踪：

    1. 直接改变属性

        ```Python
        tensor.requires_grad_(False)
        ```

    2. 将张量声明为叶子节点`.detach()`

    3. 对`.data`进行操作

    4. `with torch.no_grad()`下的代码块不被追踪

- `.backward()`计算的梯度会累加，一般需要将梯度清零

    ```Python
    x.grad.data.zero_()
    ```

## 索引问题

批量索引时，冒号和列表索引意义不同，如

```python
y = torch.tensor([1, 2])
y_hat = torch.tensor([[0.2, 0.5, 0.3], [0.4, 0.1, 0.5]])
y_hat[:, y]
y_hat[[0, 1], y]
```

结果分别为：

```python
tensor([[0.5000, 0.3000],
        [0.1000, 0.5000]])
tensor([0.5000, 0.5000])
```

**冒号索引会对每行/列进行取值操作**

**列表索引会取坐标，如样例中就是取[0, 1]与[1, 2]**

## 神经网络搭建

- 从`nn.Module`继承创建子类
```python
class MLP(nn.Module):
    def __init__(self):
        super(MLP, self).__init__()
        self.L_set = nn.Sequential(
            nn.Flatten(),
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 100),
            nn.ReLU(),
            nn.Linear(100, 10),
            nn.Softmax(1)
        )

    def forward(self, x):
        x = self.L_set(x)
        return x
```

- 利用`nn`建立神经网络层均为**类**，要么利用`nn.Sequential`建立，要么通过如下方法中的`Flatten()(x)`：
```python
class NeuralNetwork(nn.Module):
    def __init__(self):
        super(NeuralNetwork, self).__init__()
        self.flatten = nn.Flatten()

    def forward(self, x):
        x = self.flatten(x)
        return x
```

- 将模型放到GPU上需把样本与标签放入`x = x.to("cuda")`，还需把模型放入`model = model.to("cuda")`

- 检验模型精确度
```python
def test_loop(dataloader, model):
    accuracy = 0.0
    size = len(dataloader.dataset)
    with torch.no_grad():
        for X, y in dataloader:
            X = X.to("cuda")
            y = y.to("cuda")
            y_hat = model(X)
            accuracy += (y_hat.argmax(1) == y).type(torch.int64).sum().item()
    accuracy /= size
    return accuracy
```
`y_hat.argmax(1) == y`得到的是`torch.bool`需要转换成其他类型再相加，记得用`.item()`将标量从tensor转换成数字
