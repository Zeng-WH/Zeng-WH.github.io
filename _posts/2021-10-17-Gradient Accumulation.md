# Gradient Accumulation

https://towardsdatascience.com/gradient-accumulation-overcoming-memory-constraints-in-deep-learning-36d411252d01

完整题目：Gradient Accumulation: Overcoming Memory Constraints in Deep Learning

## Gradient Descent

在使用梯度下降更新模型时，一般情况下，不是在全部数据集上计算损失函数和最终梯度，而是在更小的batch上计算。当面对OOM的问题时，有时会采用减小batch size的方法，但如果batch size过小时计算出的损失和梯度无法反映整个数据集。因此会导致模型收敛变慢，甚至完全不收敛。

## Gradient Accumulation

gradient accumulation的思想非常简单。在每个mini-batch计算梯度后，并不立刻更新模型。而是在连续的mini-batch下累计梯度，在特定数目的batch后更新模型参数。

> Example: If you run a gradient accumulation with steps of 5 and batch size of 4 images, it serves almost the same purpose of running with a batch size of 20 images.

## Implementation(PyTorch)

gradient accumulation的PyTorch实现如下

```python
model.zero_grad()                                   # Reset gradients tensors
for i, (inputs, labels) in enumerate(training_set):
    predictions = model(inputs)                     # Forward pass
    loss = loss_function(predictions, labels)       # Compute loss function
    loss = loss / accumulation_steps                # Normalize our loss (if averaged)
    loss.backward()                                 # Backward pass
    if (i+1) % accumulation_steps == 0:             # Wait for several backward steps
        optimizer.step()                            # Now we can do an optimizer step
        model.zero_grad()                           # Reset gradients tensors
        if (i+1) % evaluation_steps == 0:           # Evaluate the model when we...
            evaluate_model()                        # ...have no gradients accumulated
```

`optimizer.step()`用来更新`accumulation_steps`个batches的参数；同时，`model.zero_grad()`用来重置累积的梯度。