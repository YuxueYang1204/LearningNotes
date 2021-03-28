# Pytorch经验总结

## 梯度与反向传播

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
