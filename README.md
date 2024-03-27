# 致力于实现知识图谱对接大模型

本项目目的是把自然语言的文本转换成Neo4j的 Cypher 查询语句。和另一个大家可能已经比较熟悉的场景 Text2SQL：文本转换 SQL 在形式上没有什么区别。但是知识图谱查询的发展非常缓慢，RAG目前是主流，通过文本转换为向量进行相似度查询得到最优Prompt

## Neo4j对接现状

![Diagram showing the workflow of a user asking a question, which is processed by a Cypher generating chain, resulting in a Cypher query to the Neo4j Knowledge Graph, and then an answer generating chain that provides a generated answer based on the information from the graph.](https://raw.githubusercontent.com/langchain-ai/langchain/master/templates/neo4j-cypher/static/workflow.png)

### langchain

项目地址：https://github.com/langchain-ai/langchain/tree/master/templates/neo4j-cypher

langchain已经对接了Neo4j，可以从代码中看到[neo4j_cypher](https://github.com/langchain-ai/langchain/tree/master/templates/neo4j-cypher/neo4j_cypher)/chain.py 的第19~21行文件中可以看出

```
# LLMs
cypher_llm = ChatOpenAI(model_name="gpt-4", temperature=0.0)
qa_llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0.0)
```

gpt-4用来生成查询语句，gpt-3.5-turbo用查询到的结果作为Prompt然后回答用户提出的问题

### GraphGPT

项目地址：https://github.com/varunshenoy/GraphGPT

这里用的也是gpt-4

### llama_index

项目地址：https://github.com/run-llama/llama_index/tree/v0.10.24/llama-index-integrations/tools/llama-index-tools-neo4j

案例：https://github.com/run-llama/llama_index/blob/v0.10.24/docs/docs/examples/vector_stores/Neo4jVectorDemo.ipynb

依然用的是gpt-4

### NebulaGraph-GPT

Nebula数据库，和neo4j类似

项目地址：https://github.com/wey-gu/NebulaGraph-GPT

这里使用gpt-3生成查询语句

## 本人想做的工作

经过在https://huggingface.co/的搜索，并没有找到相关模型，似乎只又chatgtp-4可做到生成查询语句

另外也没有在huggingface上搜索到比较好的数据集

但是费点时间总能出结果，找到了一个质量很好的数据集放到了huggingface:[text-to-neo4j-cypher-chinese](https://huggingface.co/datasets/Doraemon-AI/text-to-neo4j-cypher-chinese)

来源于论文 SpCQL: A Semantic Parsing Dataset for Converting Natural Language into Cypher

接下来准备微调gemma-7b，做出一个可以对接neo4j的模型