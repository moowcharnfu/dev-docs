```
##### CMake版本升级为3.12.1的方法 ###
### 参考 https://gitee.com/jiang_rongqiang/pytorch_1.8 ###
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py37_4.10.3-Linux-$(arch).sh --no-check-certificate

pip install --upgrade pip

pip uninstall torch torch-npu
pip install torch==1.8.1 -f https://download.pytorch.org/whl/torch_stable.html -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com 

# 构建torch_npu.whl
git clone https://gitee.com/ascend/pytorch.git
cd pytorch/
# 指定python版本编包方式：
bash ci/build.sh --python=3.7

# 部署到本地
pip install --upgrade torch_npu-1.8.1rc2-cp37-cp37m-linux_aarch64.whl


# lib*.so没找到
cp /root/Python-3.7.5/libpython3.7m.so /usr/lib


#验证npu是否能用(未通过)
import torch 
import torch_npu # Ascend Extension for PyTorch 

npu = torch.device('npu')
input = torch.randn([100]).to(npu)
model.to(npu)
output = model(input)
```
