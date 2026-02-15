# D041 知识图谱智能问诊问答系统|vue+django+neo4j|问诊记录|预问诊

> 完整项目收费，可联系QQ: 81040295 微信: mmdsj186011 注明从github来的，谢谢！
也可以关注我的B站： 麦麦大数据 https://space.bilibili.com/1583208775
> 

up主B站：  **麦麦大数据**
关注B站，有好处！

编号:  D041
## 视频
(需要更新）
https://www.bilibili.com/video/BV1cJzwYjEKh/
## 1 系统简介
系统简介：本系统是一个基于Vue+Django+Neo4j构建的知识图谱智能问诊问答系统，旨在为用户提供医疗疾病知识的可视化展示与智能问答服务。系统的核心功能围绕知识图谱的构建、可视化展示、疾病查询、智能问诊以及用户管理等模块展开。主要功能包括：图谱构建模块，用于将医疗数据组织成节点和关系存储到图数据库Neo4j中；图谱可视化模块，通过可视化技术展示知识图谱，支持关键词查询、节点点击和类别展示等操作；疾病查询模块，提供疾病详情的查看和筛选功能；智能问诊模块，基于知识图谱和ahocorasick算法实现疾病相关问答，记录问诊对话；以及用户管理模块，提供用户登录、注册、信息设置和权限管理功能。系统采用前后端分离架构，前端使用Vue.js框架进行开发，后端基于Django框架处理业务逻辑，数据存储在Neo4j图数据库中，确保系统高效、易用并具有良好的可视化体验。
## 2 功能设计
该系统采用典型的B/S（浏览器/服务器）架构模式，用户通过浏览器访问Vue前端界面，前端由HTML、CSS、JavaScript以及Vue.js生态系统中的组件构建，包括Element UI（用于界面组件）、ECharts（用于数据可视化）和Axios（用于前后端数据交互）。前端通过API请求与Django后端进行数据交互，后端负责业务逻辑处理，并利用Py2Neo等工具与Neo4j图数据库进行数据存储和查询。系统还包含一个独立的爬虫模块，负责从外部来源抓取医疗数据并将其导入Neo4j数据库，为知识图谱构建提供数据支撑。通过这种架构设计，系统能够实现高效的数据管理、可视化展示和智能问答功能，为用户提供便捷的医疗咨询服务。
### 2.1系统架构图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/027eddf9ce96411996fb6e1dce7c3328.png)
### 2.2 功能模块图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bc4989075a974a378c0604d57bc63bcf.png)
### 2.3 系统用例图 
普通用户用例图：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6d9805599af146188a65baf8bcdec549.png)
管理员用例图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e4282edd307e47ee8626f27d519dde08.png)
## 3 功能展示
### 3.1 图谱的构造
图谱构建是基于医疗的数据集，有8000多个疾病实体（或者说是图数据库的节点），以及检查项目、推荐药品、食物、症状和所属的科室等实体，关系方面包含了疾病症状、推荐药品、宜吃的食物，禁止吃的食物等，需求是将这些数据组织成节点和关系的数据，并且存储到图数据库neo4j中，后续基于这个构建好的疾病知识图谱开发其他功能。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/94d1a50305704006ad99ed676f006bdd.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8106a80ca2cb4371a38a042ecb21f06b.png)
完成后，在neo4j视图下查看：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5911281eaaa6405f8ba62bc0abd75a8c.png)
### 3.2 登录 & 注册
登录界面背景是一个视频，展示和本文系统主题相匹配的内容，登录和注册界面在一个界面下，通过按钮来切换，注册界面输入用户名和密码，会检查这个用户是否存在，登录界面则要检查用户名是否存在以及用户名密码是否正确：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f861b937085c42739d073ef23368de4e.png)
### 3.3 主页
如果通过校验，则可以进入主页，在主页是一个左侧菜单，右侧操作面板的布局，右上角是登录用户的头像和退出按钮：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/43a44d591baa4b08a9eac75c70fa567d.png)
### 3.3 疾病图谱的可视化
图谱的可视化主要是对医疗的数据集进行可视化展现，利用可视化技术能够进行知识图谱的前端展示，而和图数据库的链接部分放在后端，也就是前端只负责展示和发送查询请求，后端负责去图数据库进行查询，返回结果给前端。同时，可视化部分要求能够实现根据关键词查询、图形可以点击、可以按照类别展示不同节点，支持放大、缩小，拖动等操作。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fe9780221c114959bfa56491f2d5383e.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/57f4fc3735c54eb48e64976d6b079117.png)
### 3.4 疾病查询
在主页上设计了常见疾病的展示，这个展示应该是随机展示一些常见的疾病，点击疾病之后，可以点击查看疾病的详情（进入详情页面），例如某种疾病的检查方法、如何治疗、得病之后适宜吃什么食物、禁止吃什么食物、症状有哪些、疾病的成因等。另外可以通过点击科室、点击症状进入到疾病查询的页面中，在这个页面既可以根据疾病名称模糊查询，也可以筛选科室、症状查询疾病，同样的可以点击进入详情页面查看疾病的详情。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b0a93f08c9ec4dcbbe92d7709ae07dd7.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1570b2c80a214eeba1496e4463ffbfa0.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0a0d8bb3321144dcb4f28d3529555ee4.png)
### 3.5 疾病知识
系统会随机展示疾病知识，让用户登录系统的时候可以进行了解，增强疾病防护的相关知识，作为一种科普的手段。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/145450363d7a4c42a739e21cd98f2532.png)

### 3.6 智能问诊
基于构建好的知识图谱，基于ahocorasick算法设计和实现一个问答系统，可以支持疾病相关问答，比如询问某种疾病的检查方法、如何治疗、得病之后适宜吃什么食物、禁止吃什么食物、症状有哪些、疾病的成因等。问答界面需要设计的人性化，使得用户不需要培训就可以直接上手使用，可以设计成聊天的界面，同时，问诊记录也需要被记录。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/16d6672684374d15ad608b6f97abf9dd.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/75d55f570cf54f53a5a2d1853b39dfa3.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/620fc159f1e94b5580e80735223a8444.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4f6660c6ea9647c9961ba4cbddea653f.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4e80cc452b7f466b96dea8993228fbd2.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1a67f06a8735475fb658beb518976427.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/aad3f6b6ad244b9ebb8badc0fda76408.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c91ddb65ae184036aa16898c43dd03e5.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/95e70c8bc10246af9df7dde87b8c96a0.png)
### 3.7 可视化数据分析
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4e2aebceab4e4110877f074647072879.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4bfef2223e6343f89f0fbbda23d1722c.png)
### 3.8 问诊记录
用户通过智能问诊功能完成的对话记录，都会被系统保存，这个记录可以按照日期来进行归档，用户在问诊记录中，可以点击具体的问诊日期，查看当天问了什么问题，系统是如何答复的。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/afe87c4347b74be7a31df304aae4e23d.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/eb08a0bdf9d945cea69faa0d7d815ee0.png)
### 3.9 个人设置
个人设置方面包含了**用户信息修改**、**密码修改**功能。
用户信息修改中可以上传头像，完成用户的头像个性化设置，也可以修改用户其他信息。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/14c6957bb3d04ac384a681dcc92ee0ed.png)
修改密码需要输入用户旧密码和新密码，验证旧密码成功后，就可以完成密码修改。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7b7df04ec90d4d7e9bceca5d1b959b7f.png)
### 3.10 用户管理【管理员功能】
管理员可以进行用户增删改查的管理。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ce6a5a05f74d4188b09e9117f6bfc2d2.png)
### 3.11 敏感词管理 【管理员功能】
如果用户问题出发了敏感词，系统就不会回答，这部分管理员可以在这个功能模块中进行管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0cdf6dc8124542de8f7dbeef8afccbb5.png)
### 3.12 问答记录管理【管理员功能】
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ecd1046420354919b3e75c5b3e1ea99f.png)
### 3.13 权限管理【管理员功能】
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/52601ea45897416f90b6420767821853.png)

## 4程序代码
### 4.1 代码说明
代码介绍：基于知识图谱的智能问诊系统旨在通过整合大量医疗知识和症状数据，帮助用户进行智能化的问诊和初步诊断。该系统利用知识图谱存储疾病、症状、药物等医疗实体之间的关系，并通过图谱推理和计算机算法分析用户的输入症状，从而提供可能的疾病诊断建议。系统可以通过自然语言处理技术理解用户的描述，结合知识图谱中的关联信息，逐步缩小疾病范围，帮助用户更好地了解自己的健康状况。该系统适用于初步自我诊断、健康建议生成等场景，特别是在医疗资源有限的情况下，能够提供及时、便捷的医疗建议。
### 4.2 流程图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/32195d16a9384d4dbed4bdc6f0c45a19.png)

### 4.3 代码实例
```python
import networkx as nx
from typing import Dict, List

# 知识图谱初始化
class KnowledgeGraph:
    def __init__(self):
        self.graph = nx.Graph()
        
    def add_node(self, node: str, label: str):
        self.graph.add_node(node, label=label)
        
    def add_edge(self, source: str, target: str, relation: str):
        self.graph.add_edge(source, target, relation=relation)
        
    def query(self, node: str) -> Dict:
        neighbors = self.graph.neighbors(node)
        result = {}
        for neighbor in neighbors:
            edges = self.graph.get_edge_data(node, neighbor)
            for edge in edges.values():
                result[neighbor] = edge['relation']
        return result

# 智能问诊系统
class DiagnosticSystem:
    def __init__(self, kg: KnowledgeGraph):
        self.kg = kg
        self.symptoms = []
        
    def collect_symptoms(self):
        while True:
            symptom = input("请输入您的症状（或'完成'结束）：")
            if symptom.lower() == '完成':
                break
            self.symptoms.append(symptom)
    
    def diagnose(self) -> List[str]:
        possible_diseases = set()
        for symptom in self.symptoms:
            relations = self.kg.query(symptom)
            for disease, relation in relations.items():
                possible_diseases.add(disease)
        return list(possible_diseases)

    def run(self):
        print("欢迎使用智能问诊系统！")
        self.collect_symptoms()
        diseases = self.diagnose()
        if diseases:
            print("可能的疾病有：")
            for disease in diseases:
                print(f"- {disease}")
            print("建议尽快就医！")
        else:
            print("未找到任何可能的疾病，但建议您观察症状或咨询专业医生。")

# 示例知识图谱初始化
kg = KnowledgeGraph()
kg.add_node("头痛", label="症状")
kg.add_node("发烧", label="症状")
kg.add_node("感冒", label="疾病")
kg.add_node("脑膜炎", label="疾病")
kg.add_edge("头痛", "感冒", relation="症状")
kg.add_edge("头痛", "脑膜炎", relation="症状")
kg.add_edge("发烧", "感冒", relation="症状")
kg.add_edge("发烧", "脑膜炎", relation="症状")

# 运行系统
system = DiagnosticSystem(kg)
system.run()

```
