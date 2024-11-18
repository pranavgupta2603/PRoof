# Understand

```
def _get_batch_size_list(batch_size_arg: str) -> List[int]:
    if '-' in batch_size_arg:   # range
        min_b, max_b = map(int, batch_size_arg.split('-'))
        assert min_b > 0
        assert max_b >= min_b
        assert min_b & (min_b - 1) == 0
        assert max_b & (max_b - 1) == 0
        li = [min_b]
        while li[-1] < max_b:
            li.append(li[-1] * 2)
        assert li[-1] == max_b
        return li
    elif ',' in batch_size_arg:
        li = list(map(int, batch_size_arg.split(',')))
        assert all(b > 0 for b in li)
        return li
    else:   # number
        batch_size = int(batch_size_arg)
        assert batch_size > 0
        return [batch_size]
```
Processes batch size input (batch_size_arg) to support different formats:
Range format (e.g., 1-16) is parsed to generate a list doubling each step until reaching the maximum.
Comma-separated values (e.g., 1,2,4) are split into a list.
Single number is simply converted into a one-item list.
Assertions ensure only positive, power-of-2 values are accepted.

How are FLOPS calculated:
  File "/home/saisamarth/pranav/analytical/PRoof/main.py", line 126, in <module>
    ctx.run()
    
  File "/home/saisamarth/pranav/analytical/PRoof/context.py", line 111, in run
    self.model_ctx.run()
    
  File "/home/saisamarth/pranav/analytical/PRoof/context.py", line 232, in run
    self.analyze = model.analyze.Analyze(self.onnx_model)
    
  File "/home/saisamarth/pranav/analytical/PRoof/model/analyze/__init__.py", line 21, in __init__
    self.data, self.ops = onnx_model_layer_profile(self.model)
    
  File "/home/saisamarth/pranav/analytical/PRoof/model/analyze/model.py", line 124, in onnx_model_layer_profile
    node_data.flops = op.get_flops()
    
  File "/home/saisamarth/pranav/analytical/PRoof/model/analyze/op.py", line 296, in get_flops

