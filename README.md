### 部署到服务器

##### 激活虚拟环境

```shell
sudo su root
```

```shell
cd /www/chatbbj_rasa
```

```shell
source venv/bin/activate
```

##### 查找Rasa进程并关闭

```shell
ps aux | grep rasa
```

```shell
netstat -anp |grep 5005
```

```shell
kill [PID]
```

##### 启动Rasa服务

```
nohup rasa run --cors "*" &
```

### 安装

##### Rasa

```shell
pip3 install 'rasa[full]'
```

##### Spacy

```shell
pip install -U pip setuptools wheel
```

```shell
pip install -U 'spacy[cuda12x]'
```

```shell
python -m spacy download zh_core_web_trf
```

##### Cuda

- cuda_12.3.2_546.12_windows.exe

```shell
nvcc --version
```

### 常用命令

##### 查看可用命令

```shell
rasa -h
```

##### 新建并初始化项目

```shell
rasa init
```

##### 训练模型

```shell
rasa train
```

- -h, --help
- nlu
- core
- --out OUT
- --fixed-model-name FIXED_MODEL_NAME

##### 交互式学习对话

```shell
rasa interactive
```

- -h, --help
- -m MODEL, --model MODEL

可视化界面：

- http://localhost:5006/visualization.html


##### 命令行对话

```shell
rasa shell
```

- -h, --help
- nlu
- --skip-visualization
- -p PORT, --port PORT
- -m MODEL, --model MODEL
- --out OUT

查看会话开始行为

- /session_start

##### 启动服务器

```shell
rasa run --cors "*"
```

- -h, --help
- -p PORT, --port PORT
- -m MODEL, --model MODEL
- --cors "*"

##### 启动动作服务器

```shell
rasa run actions
```

- -h, --help
- -p PORT, --port PORT
- --cors "*"

##### 生成故事图表

```shell
rasa visualize
```

- -h, --help
- -d DOMAIN, --domain DOMAIN
- -s STORIES, --stories STORIES
- --out OUT
- -u NLU, --nlu NLU

##### 模型评估

```shell
rasa test
```

- -h, --help
- nlu
- core
- -m MODEL, --model MODEL

##### 数据验证

```shell
rasa data validate
```

- -h, --help
- stories
- --fail-on-warnings
- -d DOMAIN, --domain DOMAIN
- --data DATA [DATA ...]

### rasa服务器API

- localhost:5005/webhooks/rest/webhook

```json
{
  "sender": "test_user",
  "message": "are you human"
}
```

### Pipline

##### Language Models

```yaml
- name: "SpacyNLP"
  model: "zh_core_web_trf"
```

##### Tokenizers

```yaml
- name: "SpacyTokenizer"
```

##### Featurizers

```yaml
- name: RegexFeaturizer
- name: LexicalSyntacticFeaturizer
- name: CountVectorsFeaturizer
- name: CountVectorsFeaturizer
  analyzer: char_wb
  min_ngram: 1
  max_ngram: 4
```

```yaml
- name: "SpacyFeaturizer"
```

##### Intent Classifiers

```yaml
- name: FallbackClassifier
  threshold: 0.3
  ambiguity_threshold: 0.1
```

##### Entity Extractors

```yaml
- name: EntitySynonymMapper
```

##### Combined Intent Classifiers and Entity Extractors

```yaml
- name: DIETClassifier
  epochs: 100
  constrain_similarities: true
```

##### Selectors

```yaml
- name: ResponseSelector
  epochs: 100
  constrain_similarities: true
```

