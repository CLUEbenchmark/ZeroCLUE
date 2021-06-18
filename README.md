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
| **<a href="https://github.com/ymcui/Chinese-BERT-wwm">FineTuning.B</a>**        | 39.35 |61.9N   | 54.1N   | 33.6N  | 25.6N |40.5N | 50.3N |22.6N | 50.5N| 15.0N|
| **<a href="https://github.com/CLUEbenchmark/FewCLUE/tree/main/baselines/models_keras/gpt">Zero-shot.G</a>**      | 43.36N |  57.54N |  50N  | 34.4N  |  26.23N |  36.96N | 50.31N | 19.04N | 50.14N  | 65.63N  |
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

## 测试流程
    直接在测试集(test.json)上做预测; 生成的预测文件为：<task>_predict.json。如bustm_predict.json
  
## Datasets 数据集
    需要预测的测试文件地址： ./datasets/

## Baseline 基线模型
    说明：./baselines/readme.md

## Submit Examples 提交样例
    ./resources/zeroclue_submit_examples
   
## 排行榜
<a href='https://www.cluebenchmarks.com/zeroclue.html'>排行榜，及开启测评</a>

## 问题反馈
发送邮件:CLUEbenchmarks@163.com，或提交issue

## 引用
    @article{CLUEbenchmark,
      title={CLUE: A Chinese Language Understanding Evaluation Benchmark},
      author={Liang Xu, Hai Hu, Xuanwei Zhang, Lu Li, Chenjie Cao, Yudong Li, Yechen Xu, Kai Sun, Dian Yu,Cong Yu,et al.},
      journal={In Proceedings of the 28th International Conference on Computational Linguistics, pages 4762–4772.},
      year={2020}
     }
