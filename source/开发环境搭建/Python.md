# pip3
```python

pip install --upgrade pip
python -m pip install --upgrade pip
python -m pip install --upgrade pip --trusted-host pypi.org --trusted-host files.pythonhosted.org
python -m pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple


pip3 install -vvv cryptography
pip3 uninstall cryptography

pip search cffi --index-url https://pypi.tuna.tsinghua.edu.cn/simple

pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple cffi==1.17.0
pip3 install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography
pip3 install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography==42.0.2

pip3 download -i https://pypi.tuna.tsinghua.edu.cn/simple cryptography==42.0.2 -d ./
pip3 download -i https://mirror.zlgmcu.com/repository/pypi-group/simple cryptography --only-binary :all: -d ./
pip3 download -i https://mirror.zlgmcu.com/repository/pypi-group/simple cryptography==42.0.2 --only-binary :all: -d ./

pip3 install --no-cache-dir -f ./ cryptography-42.0.2-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
pip3 install -f https://mirror.zlgmcu.com/repository/pypi-group/simple cryptography-2.1.4-cp36-cp36m-manylinux1_x86_64.whl

pip3 --version
python3 --version
python3 -c "import cryptography"

which python python2 python3
```

# Selenium


# 案例
## 自动登录
```python
pip install selenium webdriver-manager -i https://pypi.tuna.tsinghua.edu.cn/simple
```
```python
#!/usr/bin/env python3

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# 设置 Chrome 浏览器选项
chrome_options = Options()
chrome_options.add_argument("--disable-extensions")  # 禁用扩展
chrome_options.add_argument("--disable-popup-blocking")  # 禁用弹窗拦截
chrome_options.add_argument("--profile-directory=Default")  # 使用默认的用户配置文件目录
chrome_options.add_argument("--ignore-certificate-errors")  # 忽略证书错误
chrome_options.add_argument("--disable-plugins-discovery")  # 禁用插件发现
chrome_options.add_argument("--incognito")  # 以无痕模式启动
chrome_options.add_argument("user_agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3")  # 设置用户代理

# 指定 ChromeDriver 的路径
chromedriver_path = r'D:\downloads\chromedriver-win64\chromedriver-win64\chromedriver.exe'

# 设置 ChromeDriver 服务
service = Service(chromedriver_path)
driver = webdriver.Chrome(service=service, options=chrome_options)

# 打开网页
driver.get("https://gitee.com/login") 

# 显式等待，直到页面元素加载完成
wait = WebDriverWait(driver, 10)

# 使用正确的 XPath 定位用户名和密码输入框
username_field = wait.until(EC.presence_of_element_located((By.XPATH, '//input[@placeholder="手机／邮箱／个人空间地址"]')))
password_field = wait.until(EC.presence_of_element_located((By.XPATH, '//input[@placeholder="请输入密码"]')))

# 输入登录凭据
username_field.send_keys('15876519422')
password_field.send_keys('leweyu2023.')  # 将 'your_password' 替换为实际密码

# 定位登录按钮并点击
login_button = wait.until(EC.element_to_be_clickable((By.XPATH, '//input[@value="登 录"]')))
login_button.click()

# 等待几秒钟以确保页面加载
time.sleep(5)

# 如有需要，可以添加更多的自动化步骤

# 关闭浏览器
driver.quit()
```

## 加班时间获取
```python
pip3 install bs4 -i https://pypi.tuna.tsinghua.edu.cn/simple
pip3 install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
pip3 install openpyxl -i https://pypi.tuna.tsinghua.edu.cn/simple
```

```python
#!/usr/bin/env python3

import pandas as pd
from bs4 import BeautifulSoup
from datetime import datetime

# 文件路径
file_path = 'D:\\overwork\\111.txt'

# 加班人信息
person_name = '李威谕'
person_code = '00001381'
overtime_note = 'am62x开发'

# 尝试读取文件内容
print("开始读取文件内容...")
try:
    with open(file_path, 'r', encoding='gbk') as file:
        html_content = file.read()
except UnicodeDecodeError:
    print("GBK 解码失败，尝试使用 UTF-8 编码...")
    try:
        with open(file_path, 'r', encoding='utf-8') as file:
            html_content = file.read()
    except UnicodeDecodeError:
        print("UTF-8 解码失败，尝试使用 ISO-8859-1 编码...")
        try:
            with open(file_path, 'r', encoding='iso-8859-1') as file:
                html_content = file.read()
        except UnicodeDecodeError:
            print("ISO-8859-1 解码失败，无法读取文件。")
            exit(1)
except FileNotFoundError:
    print(f"文件未找到: {file_path}")
    exit(1)

# 使用 BeautifulSoup 解析 HTML 内容
soup = BeautifulSoup(html_content, 'html.parser')

# 初始化数据列表
data = []

# 定位所有 NormalRow 和 AlternatingRow_ 类的行
rows = soup.find_all(class_=['NormalRow', 'AlternatingRow_'])

# 解析文件内容
print("开始解析文件内容...")
i = 0
while i < len(rows):
    start_row = rows[i]
    start_cells = start_row.find_all('td')
    start_date_str = start_cells[0].get_text(strip=True).strip()[:10]
    start_time_str = start_cells[1].get_text(strip=True).strip()

    # 将日期字符串转换为datetime对象
    date_obj = datetime.strptime(start_date_str, '%Y-%m-%d')

    # 如果日期不是周六或周日，设置开始时间为18:01
    if date_obj.weekday() not in [5, 6]:  # 5是周六，6是周日
        start_time_str = '18:01'

    # 初始化结束时间为 None
    end_date_str = None
    end_time_str = None

    # 检查下一行是否存在且日期是否与当前行相同
    if i + 1 < len(rows):
        next_row = rows[i + 1]
        next_cells = next_row.find_all('td')
        next_date_str = next_cells[0].get_text(strip=True).strip()[:10]

        # 如果日期相同，则把下一行作为结束时间
        if start_date_str == next_date_str:
            end_date_str = next_date_str
            end_time_str = next_cells[1].get_text(strip=True).strip()
            i += 2  # 跳过已处理的结束时间行
        else:
            i += 1
    else:
        i += 1

    # 如果找到了结束时间
    if end_date_str and end_time_str:
        print("")
        print(f"开始时间: {start_date_str} {start_time_str}")
        print(f"结束时间: {end_date_str} {end_time_str}")
        print("")

        try:
            start_datetime = datetime.strptime(f"{start_date_str} {start_time_str}", '%Y-%m-%d %H:%M')
            end_datetime = datetime.strptime(f"{end_date_str} {end_time_str}", '%Y-%m-%d %H:%M')

            # 检查条件并添加数据
            if (date_obj.weekday() not in [5, 6] and end_datetime.time() > datetime.strptime('19:00', '%H:%M').time()) or date_obj.weekday() in [5, 6]:
                data.append([person_name, person_code, start_datetime.strftime('%Y/%m/%d-%H:%M:%S'), end_datetime.strftime('%Y/%m/%d-%H:%M:%S'), overtime_note])
        except ValueError as e:
            print(f"日期时间格式错误: {e}")

# 打印加班记录行数
print("加班记录行数:", len(data))

# 创建 DataFrame
df = pd.DataFrame(data, columns=['加班人姓名', '加班人编码', '开始时间', '结束时间', '加班说明'])

# 保存为 Excel 文件
output_file = 'D:\\overwork\\overtime.xlsx'
if len(df) > 0:
    df.to_excel(output_file, index=False)
    print(f"加班记录已保存到: {output_file}")
else:
    print("没有加班记录被写入 Excel 文件。")
```

## 上传加班时间
```python
pip install selenium
pip install webdriver-manager
```

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
import time

# 创建 WebDriver 实例
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))

# 打开目标网站
driver.get('https://hr.zlgmcu.com/hrself/index.html#/applications/overtime/apply/new')

# 等待页面加载（根据实际情况调整时间）
time.sleep(5)

# 定位批量 Excel 表格按键并点击
try:
    # 假设批量 Excel 表格按键的 id 是 'excel-button'
    excel_button = driver.findElement(By.XPATH, '//*[contains(text(), "批量 Excel 表格")]') 
    excel_button.click()
    print("成功点击批量 Excel 表格按键")
except Exception as e:
    print("未能找到批量 Excel 表格按键:", e)

# 等待下载完成或其他操作
time.sleep(10)

# 关闭 WebDriver
driver.quit()
```