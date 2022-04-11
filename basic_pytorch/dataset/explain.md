# dataset dataloader
## collate_fn
collate_fn参数可以自定义对getitem()之后的数据的处理方式  
通过一个函数 可以对其进行各种操作 如(对sequence len维度pad到一个维度上)  
默认的collate_fn函数的作用仅仅是将数据变为tensor 其他不发生变化  
collate_fn如果用自定义函数的话 有一个参数batch 代表getitem()之后的数据  
batch的维度是[batch_size, shape*] shape*代表getitem()返回值的维度  
