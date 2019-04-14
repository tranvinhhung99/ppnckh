# Hướng dẫn tạo cấu trúc project trong Python

Xin chào, đối với các bạn đã quen dùng C/C++ có lẽ chuyển sang Python là một cực hình vào thời gian đầu, nên hôm nay mình xin giới thiệu một số cú pháp về sử dụng thư viện trong Python.

--------------------------
## Nghệ thuật dùng thư viện và package
Cú pháp căn bản trong việc thêm thư viện của Python:
```
import <Tên thư viện> #Tương đương với "#include<Tên thư viện>" của C++
```

Trong ví dụ cụ thể, mình sẽ sử dụng thư viện **math**.
```
import math
```

Ở đây, **math** hoạt động tương tự **namespace** của C/C++ (là std), nhưng thay vì cú pháp `std::` thì mình dùng `math.`. Ví dụ:
```
# <Tên thư viện>.<Tên class/hàm>
# Ex:
x = math.gcd(3, 4) #GCD: Ước chung lớn nhất.
print(x) # 1
```

Tới đây vài bạn sẽ thấy cú pháp có phần rờm rà, cứ thử tưởng tượng có những thư viện nó truy cập namespace con của con của con (Ex: `torch.nn.functional.relu`).
Và thực tế, trong **C++**, chúng ta ít khi nào viết `std::string`, `std::cout`. 
Do đó, **Python** cũng cung cấp một cách mà ta có thể viết gọn lại như `using namespace std;`

```
#from <Tên thư viện> import *
from math import *
x = gcd(3, 4)
print(x)
```
Như các bạn có thể thấy, thay vì dùng `math.gcd` mình đã rút gọn cú pháp còn `gcd`.  
Nhưng tới đây, như việc dùng `using namespace std;`, nó có sự thiếu tường minh khi tên 2 hàm / class bị trùng. Vì thế, trên thực tế, mình rất ít dùng phương pháp này, trừ khi lên máu lười :)
Do đó, Python hỗ trợ một số cú pháp nhằm giải quyết sự thiếu tường minh:

**Cách 1**:
```
#from <Tên thư viện> import <Tên hàm dùng 1>, <Tên hàm dùng 2>
from math import gcd, pow
x = gcd(3, 4)
y = pow(3, 4) # y = 3 ^ 4
print(x)
```
Ở đây, ta chọn cụ thể một số hàm mà ta dùng trong thư viện, do đó có thể tránh được sự trùng lắp. Nhưng đôi khi ta cũng cần dùng 2 hàm cùng tên trong một thư viện. Do đó, ta đến với cách kế tiếp:

**Cách 2**:
```
#import <Tên thư viện> as <Tên viết tắt>
import math as m
x = m.gcd(3, 4)
```
Ở đây, mình đã tóm tắt tên thư viện `math` lại chỉ còn `m`, giúp làm ngắn được một phần cú pháp. Bạn có thể thấy kĩ thuật này nhìn cũng không gì đặc biệt, nhưng nếu biết sử dụng một cách nhuần nhuyễn thì đây là một trong những cú pháp mình thích nhất ở **Python**
Bên dưới mình sẽ ví dụ cụ thể:
```
from matplotlib import pyplot as plt
# Thay vì phải viết như sau:
# fig = matplotlib.pyplot.figure()
# Ta tối giản code thành:
fig = plt.figure() 
```
Bằng cách viết như thế, code sẽ rõ ràng hơn việc chỉ có `fig = figure()` (Dễ dàng tra lại hàm trong thư viện nào). Nhưng cũng đủ ngắn gọn và dễ hiểu.
Ngoài ra, việc `import ... as ....` còn giúp chỉnh đổi code dễ dàng. 

Ví dụ:
```
## Code cũ:
from model.deep_learning_version_1 import Classification 

## Code mới ta muốn chỉnh lại bên trong nhưng giải thuật
## gần như không đổi. Điển hình là trong trường hợp đa hình.
from model.deep_learning_version_2 import ClassificationV2 as Classification
```

## Sắp xếp project trong Python
Bên dưới là một cây thư mục của mẫu cho các project.
```
sample_project
|--main1.py
|--main2.py
|
|--sub_module_1/
|            |----__init__.py
|            |----file_1.py
|            |----file_2.py
|            |----sub_of_sub_module/
|                               |-----some_file.py
|
|--sub_module_2/
|           |----__init__.py
|           |----file3.py
|           |----file4.py
```
Module có thể hiểu như tập hợp các hàm và lớp có gì đó liên quan với nhau.
Như trong đồ án client và socket thì có thể sắp xếp thành project python giống thế này:
```
client_server
|--main_client.py
|--main_server.py
|
|--common/
|       |--__init__.py
|       |--constants.py
|       |--helper.py
|
|--server/
|       |--__init__.py
|       |--server.py
|       |--serverGUI.py
|
|--client/
|       |--__init__.py
|       |--client.py
|       |--clientGUI.py
```

Đối với đa số các framework Python, người ta thường có những project template, bạn có thể tìm hiểu thêm:
- Pytorch project template: [Link](https://github.com/victoresque/pytorch-template)
- Keras/Tensorflow template: [Link](https://github.com/MrGemy95/Tensorflow-Project-Template)

### import từ file .py khác trong project:
Ta có thể xem mỗi **sub_module** là một thư viện, trong đó lại chứa các thư viện nhỏ là từng file `.py` hoặc các **sub_module** nhỏ hơn. Sau đó, ta có thể dùng thư viện một cách bình thường.

Ví dụ:
```
# Trong main1.py
from sub_module_1 import file1
from sub_module_2.file3 import function_x
from sub_module_1.sub_of_sub_module import some_file as xxx

# Trong cùng sub_module thì thêm thư viện khác ta cần 
# dùng cú pháp: .<tên file cùng sub-module>
# ví dụ trong file2.py
from .file1.py import function_y
```
### Về file \_\_init\_\_.py:
File `__init__.py` là một file đặc biệt của mỗi sub-module.
Mình cũng không tìm được tài liệu ghi rõ về cách sử dụng thằng này, nên mình chỉ đưa ra ví dụ để các bạn nắm được phần nào thôi.

Ví dụ bình thường ta cần viết trong `main1.py` là:
```
from sub_module_2.file3 import function_x
```
Nhưng bây giờ trong file `sub_module_2/__init__.py` ta viết:
```
from .file3 import function_x
```
Thì trong file `main1.py` ta có thể tóm thành:
```
from sub_module_2 import function_x
```
Ví dụ khác:
```
# sub_module_2/__init__.py
from .file3 import *

# main1.py
from sub_module_2 import * #import luôn tất cả file3, file4 và các hàm của file3
```

Mình hay dùng cách này để quản lí các phiên bản khác nhau của giải thuật.
Ví dụ:
```
# model/__init__.py
from .model_name import model_class_name as Main_Model

# train.py
from model import Main_Model
```
