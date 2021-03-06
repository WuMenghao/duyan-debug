#### 1、介绍

本工具用与日志记录python程序内存信息，包括内存对象数量(memory type num)，内存对象增长(memory type num increment)，以及堆内存信息(memory heap info)。

#### 2、使用方式
示例：
```python
import time

import debug_util


class Int:

    def __init__(self, value):
        self.value = value


def new_int(i1, rs1):
    a = Int(i1)
    rs1.append(a)


if __name__ == '__main__':
    ml = debug_util.memory_logger(log_file_name='test', duration=2)
    ml.add_target(debug_util.handlers.MEMORY_TYPE_NUM)
    ml.add_target(debug_util.handlers.MEMORY_TYPE_NUM_GROWTH)
    ml.add_target(debug_util.handlers.MEMORY_STACK)
    ml.start()
    rs = []
    i = 0
    while True:
        if i % 1000000 == 0:
            time.sleep(2)
        if i == 10000000:
            break
        new_int(i, rs)
        i += 1

```

查看log/test.log文件

```
=== types: 2022-04-06 15:19:54.733344 === 	
Int                1000000
function           7643
dict               4543
tuple              3687
weakref            3487
wrapper_descriptor 2308
list               2288
set                1516
method_descriptor  1392
getset_descriptor  1117
=== increments: 2022-04-06 15:19:56.057061 === 	
Int                 1601726  +1601726
function               7643     +7643
dict                   4543     +4543
tuple                  3686     +3686
weakref                3488     +3488
wrapper_descriptor     2308     +2308
list                   2288     +2288
set                    1516     +1516
method_descriptor      1392     +1392
getset_descriptor      1117     +1117
=== heap: 2022-04-06 15:19:57.694068 === 	
Partition of a set of 6196589 objects. Total size = 432261651 bytes.
 Index  Count   %     Size   % Cumulative  % Kind (class / dict of class)
     0 2016955  33 225898960  52 225898960  52 dict of __main__.Int
     1 2016955  33 112949480  26 338848440  78 __main__.Int
     2 2019998  33 56568872  13 395417312  91 int
     3   2165   0 17903984   4 413321296  96 list
     4  45089   1  6041583   1 419362879  97 str
     5  35822   1  2708928   1 422071807  98 tuple
     6  17286   0  1296554   0 423368361  98 bytes
     7   8675   0  1254272   0 424622633  98 types.CodeType
     8   8439   0  1147704   0 425770337  98 function
     9   1116   0   984664   0 426755001  99 type
<393 more rows. Type e.g. '_.more' to view.> 	
```