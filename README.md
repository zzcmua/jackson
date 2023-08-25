## 1.项目背景

		1.疾病负担的增加：随着人口老龄化和慢性病的普遍增加，医疗体系面临着巨大的压力。传统医疗模式难以满足人们日益增长的医疗需求，智能医疗机器人的出现能够有效缓解医疗资源的不足，并提高医疗效率。

2. 医疗需求多元化：现代社会医疗需求呈现出多样化和复杂化的趋势，包括不同年龄层、不同疾病类型、不同健康状况等各方面的差异，传统医疗系统往往难以满足这种多元化的需求。

   

## 2.项目配置

```
python== 3.7
torch==1.10.1+cu102
numpy==1.21.6
tqdm==4.64.1
matplotlib==3.3.4
transformers==4.25.1
pandas==1.2.4
neo4j==4.3.0
flask==2.2.2
requests==2.25.1
pyahocorasick==2.0.0
redis==4.4.2
elasticsearch==7.8.0
```

## 3.数据来源

​	1.大部分是来自网络上的一些对话数据

​	2.合作方给的一点数据

## 4.项目模型介绍

### 1.命名实体识别

![image-20230816191205516](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230816191205516.png)

我们使用的是BERT+BiLSTM+CRF

#### 使用方法：

1，安装特定版本的包

2，提供训练所需的数据，具体格式在data文件夹里有展示。但是需要自行分词。只需提供3个文件：
    train.txt,  validata.txt 和 预训练的词向量。

3, 修改config.py里的文件存路径，所有的配置都在这个文件里。

4，运行train.py文件开始训练，训练结果保存在log文件夹内。





### 2.命名实体审核

![image-20230816191355693](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230816191355693.png)

#### 使用方法：

bert_chinese_encode.py:使用bert模型对文本进行编码
train_data.csv:用来训练命名实体审核模型的数据
train.py:用来训练的代码(使用不同参数训练，通过观察损失值，准确率，精确率，召回率，F1值来判断模型什么时候最优，然后保存模型参数)
BERT_RNN.pth:模型训练完成以后保存的参数
RNN_MODEL.py:一个定义好的rnn模型(后续导入到主函数中使用)
predict.py:模型的主函数，调用rnn模型，训练好的参数等等，对提供的数据进行命名实体审核，并把审核后的数据重新保存。

### 3.GPT2

#### 使用方法：

首先我们将我们自己切割好的数据放在preprocess.py文件中进行一个数据PKl的一个转化，然后将我们的数据放在train.py中进行训练，让模型去学习到我们的对话。让模型有一个对话的功能，训练完之后我们可以运行interact.py去进行一个人机交互。

其中samples.txt文件是来获取我们和机器之间的一个历史对话，就豪比：

user:你好
chatbot:同好
user:你好不好
chatbot:你好。

### 4.simcse知识融合

![image-20230816191719444](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230816191719444.png)

#### 使用方法：

训练数据：doctor_online/simcse/train_sup.py

详细请看论文：https://readpaper.com/paper/3156636935

### 5.句子主题相关模型

![image-20230816193941333](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230816193941333.png)



## 5.代码运行

代码运行：
\* 训练部分： /doctor_online/bert_serve/train.py
\* 测试部分：/doctor_online/bert_serve/test.py

训练结果：
\* TimeSince: 200m 38s
\* Train Loss: 0.3348357994906973 | Train Acc: 0.9928783917785992
\* Valid Loss: 0.33568233743798975 | Valid Acc: 0.9933285250631085
