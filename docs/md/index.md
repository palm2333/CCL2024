# <center>CCL24-Eval对话级指代消解及关系抽取评测</center>

<!-- **[<center>👉 <font style=color:rgb(226,76,71)>点我立即报名</font> 👈</center>](https://docs.qq.com/form/page/DSWpjbFlsdU5wYXdt)** -->


- 组织者：熊逸芸、李霏、李波波、姬东鸿、滕冲（武汉大学）；费豪、吴胜琼（新加坡国立大学）
- 负责人及联系方式：李霏 [lifei_csnlp@whu.edu.cn]()
- 联系人及联系方式：熊逸芸 [yiyunxiong@whu.edu.cn]()


> - <a href="#content">任务内容</a>
> - <a href="#data">评测数据</a>
>     - <a href="#data_intro">数据介绍</a>
>     - <a href="#data_example">数据样例</a>
> - <a href="#eval">评测指标</a>
> - <a href="#schedule">评测赛程</a>
> - <a href="#register">报名方式</a>
> - <a href="#award">奖项设置</a>

<!--20231227-->



### <center>1.  <span id="content">任务内容</span></center>

在自然语言处理领域，对话系统的发展日益成熟，但对话中的指代消解和关系抽取仍然是挑战性问题，对于提升对话系统的语义理解和流畅性至关重要。对话关系抽取（Dialogue Relation Extraction，DRE）关注对话中不同论元之间的关系，可以帮助更好地理解对话者之间的交互模式，为对话系统的智能应答提供有力支持。指代现象在自然语言中广泛存在，尤其在多轮对话中更为突出，对话指代消解（Dialogue Coreference Resolution）可以为对话关系抽取任务提供更准确的上下文理解，从而提升抽取效果。我们推出对话级指代消解及关系抽取任务，期望能够促进对话系统在两个任务上的进展，从而推动其在语义理解领域的前沿研究。本次测评分为如下2个子任务：

- **指代消解**：识别对话中指向同一实体的不同表述；
- **对话关系抽取**：利用指代消解的结果，帮助识别对话中论元对之间的关系。


### <center>2.  <span id="data">评测数据</span></center>

#### 2.1  <span id="data_intro">数据介绍<span>

本次评测的数据集基于我们在NLPCC2023发布的数据集 DialogREC+: An Extension of DialogRE to Investigate How Much Coreference Helps Relation Extraction in Dialogs，为该数据集的2.0版本。除了提供原有的数据，我们还针对本次评测额外标注新的盲测集。计划提供的数据情况如下：


**<center>表1 子任务2语义角色说明</center>**
<center>
<table class="table table-bordered">
<thead>
<tr>
<th  style="text-align:center" colspan="3">原有的数据</th>
<th style="text-align:center">新的数据</th>
<th></th>
</tr>
</thead>
<tbody><tr>
<td style="text-align:center">训练集<br>（Train Set）</td>
<td style="text-align:center">验证集<br>（Dev Set）</td>
<td style="text-align:center">测试集<br>（Test Set）</td>
<td style="text-align:center">盲测集<br>（Blind Set）</td>
<td style="text-align:center">总数<br>Total</td>
</tr>
<tr>
<td style="text-align:center">1073</td>
<td style="text-align:center">358</td>
<td style="text-align:center">357</td>
<td style="text-align:center">360</td>
<td style="text-align:center">2148</td>
</tr>
</tbody></table>
</center>

#### 2.2  <span id="data_example">数据样例<span>

数据样例及说明如下。 

一条数据对应一个字典，`"dialog"`为对话列表；`"Relations"`为关系三元组信息，其中，`"x""y"`分别为subject和object，`"rid"`为关系对应id，`"r"`为关系类型，`"t"`为trigger，`"x_type"`和`"y_type"`分别为subject和object的类型，`"predicted_rid"`为预测关系id；`"mentions"`和`"clusters"`分别为对话内每条共指链中的共指词及其出现的位置；`"predicted_clusters"`为预测的共指链。在盲测集中，`"rid"`、`"r"`、`"t"`、`"mentions"`和`"clusters"`字段为空。

> ```
> {
>   "dialog": [
>    			"Speaker 1: Hey Pheebs.",
>    			"Speaker 2: Hey!",
>    			"Speaker 1: Any sign of your brother?",
>    			"Speaker 2: No, but he's always late.",
>    			"Speaker 1: I thought you only met him once?",
>    			"Speaker 2: Yeah, I did. I think it sounds y'know big sistery, y'know, 'Frank's always late.'",
>    			"Speaker 1: Well relax, he'll be here."],
>   "Relations":[{"x": "Speaker 2",
>    	          "y": "Frank",
>    	          "rid": [16],
>    	          "r": ["per:siblings"],
>    	          "t": ["brother"],
>    	          "x_type": "PER",
>     	          "y_type": "PER",
>     	          "predicted_rid":[]}],
>   "clusters": [[[60,72],[92,94],[143,146],[223,228],[267,269]], 	 
>                [[15,21],[22,31],[60,64],[73,82],[130,133],[152,151],[169,170],[176,177]], 	             
>                [[0,9],[37,46],[109,118],[120,121],[194,195],[214,215],[244,253]]],
>   "mentions": [["your brother","he","him","Frank","he"],       		 
>                ["Pheebs","Speaker 2","your","Speaker 2", > "you","Speaker 2","I","I"], 
>              	["Speaker 1","Speaker 1","Speaker 1","I","y","y","Speaker 1"]],
>   "predicted_clusters":[] 
>   }
>   ```



### 3.  <span id="eval">评价标准<span>

#### 3.1  子任务1：指代消解

输入对话，参赛者需要预测对话中的多条指代链，预测结果保存在`"predicted_clusters"`字段中，格式同`"clusters"`。

我们使用官方 CoNLL-2012 评估脚本计算标准 MUC、B<sup>3</sup> 和 CEAF<sub>φ4</sub> 指标的精确度、召回率和 F1值。以上三个指标的平均F1值作为参赛者在子任务一上的最终指标结果。

#### 3.2  子任务2：关系抽取

参赛者需要根据对话预测论元对之间的关系，即`"predicted_rid"`字段，需要利用**子任务1的指代链**帮助理解对话。

主要关注Precision（P）、Recall（R）和F1值（F1）三个指标。

#### 3.3  综合评价标准

1. 子任务1的最终得分为平均F1值；

2. 取F1值作为子任务2的最终得分；

3. 两个子任务的平均分将作为整个任务的综合得分。


### 4.  <span id="schedule">评测赛程<span>
**<center>表3 评测赛事日程</center>**
<table class="table table-bordered">
<thead>
<tr>
<th>时间</th>
<th>事项</th>
</tr>
</thead>
<tbody><tr>
<td>2024年3月-2024年5月</td>
<td>开放报名</td>
</tr>
<tr>
<td>2024年3月20日</td>
<td>训练集、验证集、测试集发布</td>
</tr>
<tr>
<td>2024年5月20日</td>
<td>盲测集发布</td>
</tr>
<tr>
<td>2024年5月23日</td>
<td>参赛队提交盲测集结果</td>
</tr>
<tr>
<td>2024年5月30日</td>
<td>公布参赛队伍成绩和排名</td>
</tr>
<tr>
<td>2024年5月31日</td>
<td>评测任务结束</td>
</tr>
<tr>
<td>2024年6月</td>
<td>提交中文或英文技术报告</td>
</tr>
<tr>
<td>2024年7月</td>
<td>评测论文审稿&评测论文录用通知</td>
</tr>
<tr>
<td>2024年7月</td>
<td>评测论文Camera-Ready版提交</td>
</tr>
<tr>
<td>2024年7月</td>
<td>评测论文Camera-Ready版提交</td>
</tr>
<tr>
<td>2024年8月</td>
<td>CCL 2024评测研讨会</td>
</tr>
</tbody></table>

（以上时间可能还会有调整，请关注网站最新消息）

### 5.  <span id="register">报名方式<span>

请关注网站最新消息

### 6.  <span id="award">奖项设置<span>

评测将设置一、二、三等奖，由中国中文信息学会为获奖队伍颁发荣誉证书。

