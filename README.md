# ZeroCLUE
零样本学习测评基准，中文版


    零样本学习是AI识别方法之一。 简单来说就是识别从未见过的数据类别，即训练的分类器不仅仅能够识别出训练集中已有的数据类别，
    还可以对于来自未见过的类别的数据进行区分。 这是一个很有用的功能，使得计算机能够具有知识迁移的能力，并无需任何训练数据，
    很符合现实生活中海量类别的存在形式。
    GPT系列模型，除了展示了模型只提供提示或示例，而不进行任何参数更新情况下的学习能力。

## 任务描述和原始数据量统计
| Corpus   | Train     | Dev  |Test Public| Test Private | Num Labels| Unlabeled| Task | Metric | Source |
| :----:| :----:  |:----:  |:----:  |:----:  |:----:  |:----:  |:----:  |:----:  |:----:  |
|   | Single |Sentence | Tasks  |
|   EPRSTMT    | 32 | 32 | 610 | 753 | 2 | 19565 | SntmntAnalysis | Acc | E-CommrceReview |
|   CSLDCP    | 536 |536   | 1784| 2999 | 67 | 18111 | LongTextClassify | Acc |AcademicCNKI |
|   TNEWS    | 240 | 240 |2010| 1500 | 15 |20000| ShortTextClassify | Acc |NewsTitle |
|    IFLYTEK   | 928 | 690  | 1749  | 2279 | 119  | 7558 | LongTextClassify| Acc |AppDesc |
|     | Sentence | Pair | Tasks |
|    OCNLI   | 32  | 32  |  2520 |  3000 | 3  | 20000 | NLI  |  Acc | 5Genres |
|    BUSTM   | 32 | 32  | 1772 | 2000  | 2 | 4251|SemanticSmlarty | Acc | AIVirtualAssistant |
|   |Reading |Comprhnsn |Tasks |
|     CHID  | 42 |  42 | 2002 | 2000  | 7 | 7585 |  MultipleChoice,idiom | Acc  | Novel,EssayNews |
|     CSL  | 32 |  32 | 2828 | 3000 | 2 | 19841 | KeywordRecogntn| Acc | AcademicCNKI|
|     CLUEWSC  | 32 | 32  |  976 | 290  | 2 | 0| CorefResolution  | Acc | ChineseFictionBooks 

    EPRSTMT:电商评论情感分析；CSLDCP：科学文献学科分类；TNEWS:新闻分类；IFLYTEK:APP应用描述主题分类；
    OCNLI: 自然语言推理；BUSTM: 对话短文本匹配；CHID:成语阅读理解；CSL:摘要判断关键词判别；CLUEWSC: 代词消歧
    EPRSTMT,CSLDCP,BUSTM 为新任务；其他任务（TNEWS,CHID,IFLYTEK,OCNLI,CSL,CLUEWSC）来自于CLUE benchmark，部分数据集做了新的标注。


## 实验结果
实验设置：训练集和验证集使用32个样本，或采样16个，测试集正常规模。基础模型使用RoBERT12层chinese_roberta_wwm_ext（GPT系列除外）。

| 模型   | score     | eprstmt  | bustm  | ocnli   | csldcp   | tnews | wsc | ifytek| csl | chid  |
| :----:| :----:  | :----: |:----: |:----: |:----: |:----: |:----: |:----: |:----: |:----: |
| **<a href="https://arxiv.org/abs/2004.05986">Human</a>**        | 82.49 |90.0N  | 88.0N    |  90.3N  | 68.0N |71.0N | 98.0N | 66.0N |  84.0N|  87.1N|
| **<a href="https://github.com/CLUEbenchmark/ZeroCLUE">FineTuning.B</a>**        | 39.35 |61.9N   | 54.1N   | 33.6N  | 25.6N |40.5N | 50.3N |22.6N | 50.5N| 15.0N|
| **<a href="https://github.com/CLUEbenchmark/ZeroCLUE">Zero-shot.G</a>**      | 43.36N |  57.54N |  50N  | 34.4N  |  26.23N |  36.96N | 50.31N | 19.04N | 50.14N  | 65.63N  |
| **<a href="https://arxiv.org/abs/2005.14165">Zero-shot.R</a>**      | 44.61N |  85.2N |   50.6N | 40.3N | 12.6N  |   25.3N  | 50.0N | 27.7N |  52.2N |  57.6N |
| <a href="https://github.com/CLUEbenchmark/FewCLUE/tree/main/baselines/models_keras/pet">PET</a>      | 57.36 | 87.2N | 64.0  | 43.9N | 56.9N |53.7N  | 59.2N| 35.1N | 55.0N | 61.3N |
| <a href="https://github.com/CLUEbenchmark/FewCLUE/tree/main/baselines/models_keras/ptuning">PtuningB</a>      | 51.81| 88.5N | 65.4  | 35.0N | 44.4N |  48.2N  | 51.0N | 32.0N| 50.0N | 57.6N |
| <a href="https://arxiv.org/pdf/2009.07118.pdf">PtuningGPT</a>      | 46.44| 75.65N  | 54.9N   | 35.75N  | 33.69N  |  45.3N   | 49.0N | 24.0N | 53.5N  | 13.7N  |
| <a href='https://github.com/CLUEbenchmark/FewCLUE/tree/main/baselines/models_pytorch/EFL'>EFL</a>     |53.4 | 85.6N |  67.6N | 67.5N | 46.7N | 53.5N  |   54.2N  | 44.0N | 61.6N |  28.2N |
| <a href="https://github.com/CLUEbenchmark/FewCLUE/tree/main/baselines/models_pytorch/LM-BFF">LM-BFF | 55.79 | 84.59 | 54.06 | 43.10 |  53.64 |  56.27 | 51.84 | 46.14 | 51.16 | 61.3 |


    Human: 人类测评成绩；FineTuning: 直接下游任务微调；PET:Pattern Exploiting Training(完形填空形式); 
    Ptuning: 自动构建模板; Zero-shot: 零样本学习；EFL:自然语言推理形式; ADAPET:PET改进版，带正确标签条件优化
    FineTuning.B:FineTuningBert; FineTuningR:FineTuningRoberta; PtuningB:Ptuning_RoBERTa; PtuningGPT:Ptuning_GPT; 
    Zero-shot.R，采用chinese_roberta_wwm_ext为基础模型的零样本学习；Zero-shot.G，GPT系列的零样本学习；N”，代表已更新；
    报告的数字是每一个任务的公开测试集(test_public.json)上的实验效果；CLUE榜单已经可以提交；
    由于CHID还在继续实验中，暂时未将CHID的分数纳入到最终的分数(Score）中。
   <a href='https://github.com/huawei-noah/Pretrained-Language-Model/tree/master/NEZHA-Gen-TensorFlow'>使用的GPT模型: NEZHA-Gen</a>

## Datasets 数据集
    需要预测的测试文件地址： ./datasets/

## Baseline 基线模型
    说明：./baselines/readme.md

## 测试流程
    使用验证集(dev.json)做验证，在测试集(test.json)上做预测，并生成的预测文件。格式为：<task>_predict.json，如：bustm_predict.json
  
## Submit Examples 提交样例
    ./resources/zeroclue_submit_examples
   
## 排行榜
<a href='https://www.cluebenchmarks.com/zeroclue.html'>排行榜，及开启测评</a>

 
## 数据集介绍

####   分类任务 Single Sentence Tasks
##### 1. EPRSTMT（EPR-sentiment）  电商产品评论情感分析数据集  E-commerce Product Review Dataset for Sentiment Analysis
```  电商产品评论的情感分析，根据评论来确定情感的极性，正向或负向。
     数据量：训练集（32），验证集（32），公开测试集（610），测试集（753），无标签语料（19565）
     
     例子：
     {"id": 23, "sentence": "外包装上有点磨损，试听后感觉不错", "label": "Positive"}
     每一条数据有三个属性，从前往后分别是 id,sentence,label。其中label标签，Positive 表示正向，Negative 表示负向。
```

##### 2. CSLDCP  中文科学文献学科分类数据集     
```  
     中文科学文献学科分类数据集，包括67个类别的文献类别，这些类别来自于分别归属于13个大类，范围从社会科学到自然科学，文本为文献的中文摘要。
     数据量：训练集（536），验证集（536），公开测试集（1784），测试集（2999），无标签语料（67）
     
     例子：
     {"content": "通过几年的观察和实践，初步掌握了盆栽菊花的栽培技术及方法，并进行了总结，以满足人们对花卉消费的需求，提高观赏植物的商品价值，为企业化生产的盆菊提供技术指导。",
     "label": "园艺学", "id": 1770}
     {"content": "GPS卫星导航定位精度的高低很大程度上取决于站星距离(即伪距)的测量误差.载波相位平滑伪距在保证环路参数满足动态应力误差要求的基础上。。。本文详细论述了载波相位平滑伪距的原理和工程实现方法,并进行了仿真验证.", 
     "label": "航空宇航科学与技术", "id": 979}

     每一条数据有三个属性，从前往后分别是 id,sentence,label。其中label标签，Positive 表示正向，Negative 表示负向。
```


##### 3.TNEWS 今日头条中文新闻（短文本）分类数据集 Toutiao Short Text Classificaiton for News
     该数据集来自今日头条的新闻版块，共提取了15个类别的新闻，包括旅游、教育、金融、军事等。
```  数据量：训练集（240），验证集（240），公开测试集（2010），测试集（1500），无标签语料（20000）
     
     例子：
     {"label": "102", "label_des": "news_entertainment", "sentence": "江疏影甜甜圈自拍，迷之角度竟这么好看，美吸引一切事物"}
     每一条数据有三个属性，从前往后分别是 分类ID，分类名称，新闻字符串（仅含标题）。
```

##### 4.IFLYTEK 长文本分类数据集 Long Text classification
    该数据集关于app应用描述的长文本标注数据，包含和日常生活相关的各类应用主题，共119个类别："打车":0,"地图导航":1,"免费WIFI":2,"租车":3,….,"女性":115,"经营":116,"收款":117,"其他":118(分别用0-118表示)。
``` 数据量：训练集（928），验证集（690），公开测试集（1749），测试集（2279），无标签语料（7558）
    
    例子：
    {"label": "110", "label_des": "社区超市", "sentence": "朴朴快送超市创立于2016年，专注于打造移动端30分钟即时配送一站式购物平台，商品品类包含水果、蔬菜、肉禽蛋奶、海鲜水产、粮油调味、酒水饮料、休闲食品、日用品、外卖等。朴朴公司希望能以全新的商业模式，更高效快捷的仓储配送模式，致力于成为更快、更好、更多、更省的在线零售平台，带给消费者更好的消费体验，同时推动中国食品安全进程，成为一家让社会尊敬的互联网公司。,朴朴一下，又好又快,1.配送时间提示更加清晰友好2.保障用户隐私的一些优化3.其他提高使用体验的调整4.修复了一些已知bug"}
    每一条数据有三个属性，从前往后分别是 类别ID，类别名称，文本内容。
```

#### Sentence Pair Tasks 
##### 5.OCNLI 中文原版自然语言推理数据集 Original Chinese Natural Language Inference
    OCNLI，即原生中文自然语言推理数据集，是第一个非翻译的、使用原生汉语的大型中文自然语言推理数据集。
    数据量：训练集（32），验证集（32），公开测试集（2520），测试集（3000），无标签语料（20000）
    
    例子：
    {
    "level":"medium",
    "sentence1":"身上裹一件工厂发的棉大衣,手插在袖筒里",
    "sentence2":"身上至少一件衣服",
    "label":"entailment","label0":"entailment","label1":"entailment","label2":"entailment","label3":"entailment","label4":"entailment",
    "genre":"lit","prem_id":"lit_635","id":0
    }

##### 6.BUSTM 小布助手对话短文本匹配数据集 XiaoBu Dialogue Short Text Matching 
    对话短文本语义匹配数据集，源于小布助手。它是OPPO为品牌手机和IoT设备自研的语音助手，为用户提供便捷对话式服务。
    意图识别是对话系统中的一个核心任务，而对话短文本语义匹配是意图识别的主流算法方案之一。要求根据短文本query-pair，预测它们是否属于同一语义。
    
    数据量：训练集（32），验证集（32），公开测试集（1772），测试集（2000），无标签语料（4251）
    例子：
    {"id": 5, "sentence1": "女孩子到底是不是你", "sentence2": "你不是女孩子吗", "label": "1"}
    {"id": 18, "sentence1": "小影,你说话慢了", "sentence2": "那你说慢一点", "label": "0"}

#### Reading Comprehension Tasks 
##### 7.ChID 成语阅读理解填空 Chinese IDiom Dataset for Cloze Test
    以成语完形填空形式实现，文中多处成语被mask，候选项中包含了近义的成语。 https://arxiv.org/abs/1906.01265
    数据量：训练集（42），验证集（42），公开测试集（2002），测试集（2000），无标签语料（7585）
    
    例子：
    {"id": 1421, "candidates": ["巧言令色", "措手不及", "风流人物", "八仙过海", "平铺直叙", "草木皆兵", "言行一致"],
    "content": "当广州憾负北控,郭士强黯然退场那一刻,CBA季后赛悬念仿佛一下就消失了,可万万没想到,就在时隔1天后,北控外援约瑟夫-杨因个人裁决案(拖欠上一家经纪公司的费用),
    导致被禁赛,打了马布里一个#idiom#,加上郭士强带领广州神奇逆转天津,让...", "answer": 1}

##### 8.CSL 论文关键词识别 Keyword Recognition
    中文科技文献数据集(CSL)取自中文论文摘要及其关键词，论文选自部分中文社会科学和自然科学核心期刊，任务目标是根据摘要判断关键词是否全部为真实关键词（真实为1，伪造为0）。
    数据量：训练集（32），验证集（32），公开测试集（2828），测试集（3000），无标签语料（19841）
    
    例子： 
    {"id": 1, "abst": "为解决传统均匀FFT波束形成算法引起的3维声呐成像分辨率降低的问题,该文提出分区域FFT波束形成算法.远场条件下,
    以保证成像分辨率为约束条件,以划分数量最少为目标,采用遗传算法作为优化手段将成像区域划分为多个区域.在每个区域内选取一个波束方向,
    获得每一个接收阵元收到该方向回波时的解调输出,以此为原始数据在该区域内进行传统均匀FFT波束形成.对FFT计算过程进行优化,降低新算法的计算量,
    使其满足3维成像声呐实时性的要求.仿真与实验结果表明,采用分区域FFT波束形成算法的成像分辨率较传统均匀FFT波束形成算法有显著提高,且满足实时性要求.",
     "keyword": ["水声学", "FFT", "波束形成", "3维成像声呐"], "label": "1"}
     
    每一条数据有四个属性，从前往后分别是 数据ID，论文摘要，关键词，真假标签。

##### 9.CLUEWSC  WSC Winograd模式挑战中文版
    Winograd Scheme Challenge（WSC）是一类代词消歧的任务，即判断句子中的代词指代的是哪个名词。题目以真假判别的方式出现，如：  
    句子：这时候放在[床]上[枕头]旁边的[手机]响了，我感到奇怪，因为欠费已被停机两个月，现在[它]突然响了。需要判断“它”指代的是“床”、“枕头”，还是“手机”？
    从中国现当代作家文学作品中抽取，再经语言专家人工挑选、标注。  
    
    数据量：训练集（32），验证集（32），公开测试集（976），测试集（290），无标签语料（0）
    例子：  
     {"target": 
         {"span2_index": 37, 
         "span1_index": 5, 
         "span1_text": "床", 
         "span2_text": "它"}, 
     "idx": 261, 
     "label": "false", 
     "text": "这时候放在床上枕头旁边的手机响了，我感到奇怪，因为欠费已被停机两个月，现在它突然响了。"}
     "true"表示代词确实是指代span1_text中的名词的，"false"代表不是。 


## 问题反馈
发送邮件:CLUEbenchmarks@163.com，或提交issue


## 引用
    @article{CLUEbenchmark,
      title={CLUE: A Chinese Language Understanding Evaluation Benchmark},
      author={Liang Xu, Hai Hu, Xuanwei Zhang, Lu Li, Chenjie Cao, Yudong Li, Yechen Xu, Kai Sun, Dian Yu,Cong Yu,et al.},
      journal={In Proceedings of the 28th International Conference on Computational Linguistics, pages 4762–4772.},
      year={2020}
     }
