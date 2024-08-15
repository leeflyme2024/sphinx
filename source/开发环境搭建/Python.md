# pip3
```python
pip install --upgrade pip
pip3 install -vvv cryptography
pip3 uninstall cryptography

pip search cffi --index-url https://pypi.tuna.tsinghua.edu.cn/simple

pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple cffi==1.17.0
pip3 install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography
pip3 install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography==42.0.2

pip3 download -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography==42.0.2 -d ./
pip3 install --no-cache-dir -f ./ cryptography-42.0.2-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
pip3 install -f https://mirror.zlgmcu.com/repository/pypi-group/simple cryptography-2.1.4-cp36-cp36m-manylinux1_x86_64.whl

python3 --version
python3 -c "import cryptography"
```