源地址：https://github.com/PaddlePaddle/PaddleHub/blob/release/v2.1/docs/docs_ch/get_start/linux_quickstart.md

## python与Paddle环境搭建

直接执行下列脚本即可

```bash
sudo yum install wget;
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3- 2021.05-Linux-x86_64.sh;
sh Anaconda3-2021.05-Linux-x86_64.sh;
user=`who am i| awk -F'  *' '{print $1}'`;
if [ "$user" = "root" ]
then 
	path='/root'
else
	path='/home/user'
fi
sed -i '1iexport PATH="~/anaconda3/bin:$PATH"' ${path}/.bashr;
source ${path}/.bash_profile;
haveENV=`conda info --envs | grep environments`;
if [ "$haveENV" ]
then	echo "true"
else	
	echo "conda environments error"
	exit
fi
conda create --name paddle_env python=3.8 --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
source activate
conda activate paddle_env
pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple
pip install paddlehub -i https://mirror.baidu.com/pypi/simple
```

### 安装python环境

1. 下载Anaconda

   首先安装wget：

```shell
sudo apt-get install wget  # Ubuntu
sudo yum install wget  # CentOS
```

​		然后使用wget从清华源上下载Anaconda：

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.05-Linux-x86_64.sh
#报错，请尝试在后面添加 --no-check-certificate
```

​		下载完成后安装Anaconda：

```shell
sh Anaconda3-2021.05-Linux-x86_64.sh
```

2. 将conda加入环境变量

​		先验证是否能识别conda命名：

```shell
conda info --envs
```

​		不能识别conda命令，则通过终端打开`~/.bashrc`文件，并向其第一行添加conda环境变量：

```shell
vim ~/.bashr
# export PATH="~/anaconda3/bin:$PATH"  
```

​		更新环境变量：

```shell
# 添加完环境变量后通过下面的代码更新环境变量后再次验证即可通过
source ~/.bash_profile
```

### 创建conda环境

```shell
conda create --name paddle_env python=3.8 --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```

激活刚创建的conda环境：

```shell
# 激活paddle_env环境
conda activate paddle_env
# 如果提示 initialize your shell 则先执行下面的命令
source activate
```

在刚激活的环境中安装paddle：

```shell
pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple
```

继续在paddle_env环境中安装paddlehub：

```shell
pip install paddlehub -i https://mirror.baidu.com/pypi/simple
```

### 对话模型

参考网址：https://www.paddlepaddle.org.cn/hubdetail?name=plato-mini&en_category=TextGeneration

安装模型：

```shell
# hub命令需在conda环境中运行
hub install plato-mini==1.0.0
```

开启服务：

```shell
hub serving start -m plato-mini -p 8866 # 在线测试，关闭连接后会自动关闭
nohup hub serving start -m plato-mini -p 8866 # 后台进行，关闭连接后仍然在进行

#结束服务
netstat -anp |grep 8866
kill 进程号
```

在防火墙处开发端口：

<img src="https://img-blog.csdnimg.cn/3b8a9818468d4b38982fccb70d45eb04.png" style="zoom:80%;" />

```python
# 单次交互模式 测试
import requests
import json

url = "http://101.33.226.127:8866/predict/plato-mini" # 根据实际更换IP与端口
headers = {"Content-Type": "application/json"}

while 1:
    keyword = input("")
    texts = [[keyword]]
    data = {"data": texts}
    # 发送post请求，content-type类型应指定json方式，url中的ip地址需改为对应机器的ip
    # 指定post请求的headers为application/json方式
    r = requests.post(url=url, headers=headers, data=json.dumps(data))
    print(r.json())
```

```python
# 连续交互模式
import paddlehub as hub

model = hub.Module(name='plato-mini')
with model.interactive_mode(max_turn=3):
    while True:
        human_utterance = input("[Human]: ").strip()
        robot_utterance = model.predict(human_utterance)[0]
        print("[Bot]: %s"%robot_utterance)
```

关闭服务：

```shell
hub serving stop --port 8866
```

### 情感分析模型

1. 三分类的模型网址：https://www.paddlepaddle.org.cn/hubdetail?name=emotion_detection_textcnn&en_category=SentimentAnalysis

```shell
# 先激活环境：conda activate paddle_env
hub install emotion_detection_textcnn==1.2.0
```

```shell
hub serving start -m emotion_detection_textcnn -p 8867 # 根据实际设置端口
nohup hub serving start -m emotion_detection_textcnn -p 8867 # 后台进行，关闭连接后仍然在进行
```

```python
import requests
import json

# 待预测数据
text = ["今天天气真好", "湿纸巾是干垃圾", "别来吵我"]

# 设置运行配置
# 对应本地预测emotion_detection_textcnn.emotion_classify(texts=text, batch_size=1, use_gpu=True)
data = {"texts": text, "batch_size": 1, "use_gpu":False}

# 指定预测方法为emotion_detection_textcnn并发送post请求，content-type类型应指定json方式
# HOST_IP为服务器IP
url = "http://101.33.226.127:8867/predict/emotion_detection_textcnn" # 根据实际设置IP与端口
headers = {"Content-Type": "application/json"}
r = requests.post(url=url, headers=headers, data=json.dumps(data))

# 打印预测结果
print(json.dumps(r.json(), indent=4, ensure_ascii=False))
```

2. 二分类的参考网址（准确率较高）https://www.paddlepaddle.org.cn/hubdetail?name=senta_bilstm&en_category=SentimentAnalysis

   注意：情感分类的模型在同一环境进行部署会起冲突，所以emotion_detection_textcnn与senta_bilstm不能同时部署（可以考虑docker）

```shell
# 先激活环境：conda activate paddle_env
hub install senta_bilstm==1.2.0
# 卸载 hub uninstall senta_bilstm==1.2.0
```

```shell
hub serving start -m senta_bilstm -p 8868 # 根据实际设置端口
nohup hub serving start -m senta_bilstm -p 8868 # 后台进行，关闭连接后仍然在进行
```

```shell
import requests
import json

# 待预测数据
text = ["今天天气真好", "湿纸巾是干垃圾", "别来吵我"]

# 设置运行配置
# 对应本地预测senta_bilstm.sentiment_classify(texts=text, batch_size=1, use_gpu=True)
data = {"texts": text, "batch_size": 1, "use_gpu":False}

# 指定预测方法为senta_bilstm并发送post请求，content-type类型应指定json方式
# HOST_IP为服务器IP
url = "http://101.33.226.127:8868/predict/senta_bilstm"
headers = {"Content-Type": "application/json"}
r = requests.post(url=url, headers=headers, data=json.dumps(data))

# 打印预测结果
print(json.dumps(r.json(), indent=4, ensure_ascii=False))
```

#### 语音合成

英文版：https://www.paddlepaddle.org.cn/hubdetail?name=deepvoice3_ljspeech&en_category=TextToSpeech

云服务器内存要求：4GB以上

```shell
# 安装 ruamel
pip install ruamel.yaml
# 安装 git
yum -y install git
# 下载安装相关依赖项
git clone -b v0.1.0 https://gitee.com/paddlepaddle/Parakeet
# 切换目录
cd Parakeet/
# 安装相关依赖项
pip install -e .
# 更新至 6以上
pip install tqdm --upgrade
# 安装libsndfile
yum install libsndfile
```

```shell
hub install deepvoice3_ljspeech==1.0.0
```

```shell
hub serving start -m transformer_tts_ljspeech
```

中文女士：https://www.paddlepaddle.org.cn/hubdetail?name=fastspeech2_baker&en_category=TextToSpeech

云服务器内存要求：4GB以上

### 语音识别

可识别中英文：https://www.paddlepaddle.org.cn/hubdetail?name=u2_conformer_wenetspeech&en_category=AutomaticSpeechRecognition

云服务器内存要求：4GB以上

```bash
hub serving start -m u2_conformer_wenetspeech
```

### 人脸关键点识别

https://www.paddlepaddle.org.cn/hubdetail?name=face_landmark_localization&en_category=KeyPointDetection

```
hub install face_landmark_localization==1.0.2
hub serving start -m face_landmark_localization
```

