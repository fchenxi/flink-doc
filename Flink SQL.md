---


---

<p>SQL Impl in Flink</p>
<p>跟了下Flink Table里sql的实现，flink sql的实现比较简单，一句话概述就是：借助Apache Calcite做了sql解析、逻辑树生成的过程，得到Calcite的RelRoot类，生成flink的Table，Table里的执行计划会转化成DataSet的计算，经历物理执行计划优化等步骤。</p>
<p>类比Spark SQL，Calcite代替了大部分Spark SQL Catalyst的工作(Catalyst还包括了Tree/Node的定义，这部分代码Flink也’借鉴’来了)。两者最终是计算一颗逻辑执行计划树，翻译成各自的DataSet(Spark 2.0引入Dataset并统一DataFrame，隐藏RDD到引擎内部这层，类似于执行层内部的物理执行节点)。</p>
<p>Calcite Usage</p>
<p>最新Flink代码里，在flink-table工程里，使用1.7版本的calcite-core。</p>
<p>大致的执行过程如下：</p>
<p>TableEnvironment.sql()为调用入口<br>
类似Calcite的PlannerImpl，flink实现了个FlinkPlannerImpl，执行parse(sql)，validate(sqlNode)，rel(sqlNode)操作<br>
生成Table</p>
<p>参考<br>
[1] <a href="http://blog.csdn.net/pelick/article/details/51613931">http://blog.csdn.net/pelick/article/details/51613931</a></p>

