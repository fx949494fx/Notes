### 桑基图（Sankey）
#### 使用Python Plotly绘制桑基图教程
桑基图（Sankey Diagram）是一种用于表示数据流的图表，它显示了不同实体之间的能量、材料、成本或其他资源的流动。Plotly是一个交互式图表库，支持多种编程语言，包括Python。
#### 安装Plotly
在开始之前，请确保已经安装了Plotly库。如果尚未安装，可以通过以下命令进行安装：
```bash
pip install plotly
```
#### 适用于绘制桑基图的数据格式
桑基图需要特定的数据格式，主要包括两个部分：节点（nodes）和链接（links）。
##### 节点（Nodes）
节点是桑基图中的实体，例如不同的部门、国家或过程。节点通常以列表的形式给出，每个节点包含一个唯一的标识符。
```python
nodes = [
    {'label': 'Node1', 'group': 1},
    {'label': 'Node2', 'group': 2},
    {'label': 'Node3', 'group': 1},
    # 添加更多节点...
]
```
##### 链接（Links）
链接表示节点之间的流动，通常以列表的形式给出，每个链接包含源节点、目标节点和流量值。
```python
links = [
    {'source': 'Node1', 'target': 'Node2', 'value': 10},
    {'source': 'Node2', 'target': 'Node3', 'value': 5},
    # 添加更多链接...
]
```
##### Nodes的设置
在Plotly中设置节点，你需要定义一个包含所有节点信息的列表，并在创建桑基图时传递给`go.Sankey`函数。
```python
# 导入plotly
import plotly.graph_objects as go
# 设置nodes信息
sankey_data = go.Sankey(
    node=dict(
        pad=15,
        thickness=20,
        line=dict(color="black", width=0.5),
        label=[n['label'] for n in nodes],  # 节点标签
        color="blue"  # 节点颜色
    )
)
```
##### Links的设置
设置链接时，需要定义一个包含所有链接信息的列表，并在创建桑基图时传递给`go.Sankey`函数。
```python
# 设置links信息
sankey_data.add(
    link=dict(
        source=[l['source'] for l in links],  # 源节点
        target=[l['target'] for l in links],  # 目标节点
        value=[l['value'] for l in links]  # 流量值
    )
)
```
##### 桑基图的绘制
将节点和链接信息设置好后，可以使用Plotly的`layout`和`show`方法来绘制桑基图。
```python
fig = go.Figure(data=sankey_data)
fig.update_layout(title_text="桑基图示例", font_size=10)
fig.show()
```
##### 参考资料
- Plotly官方文档: [https://plotly.com/python/](https://plotly.com/python/)
- Plotly桑基图示例: [https://plotly.com/python/sankey-diagram/](https://plotly.com/python/sankey-diagram/)
