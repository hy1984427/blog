title: 使用Mind Map写Test Case
date: 2012-04-23
categories:
- Test
tags:
- Mind Map
- Test Case
- Acceptance Criteria
- AC
comments: true
---

在之前的项目中，Acceptance Criteria （AC）的描述会存放在Jira Description中；在当前项目中，除了在Jira中描述AC外，我们会使用Mind Map来写AC，以便形成测试用例Test Case，并标注测试结果和注测试代码覆盖，用着效果不错。

具体格式可以参考[这里](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/PageName_FeatureName_StoryID.mm)。

![Mind Map中的Test Case](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/images/test_case_in_mindmap.png)

Mind Map中的AC虽然没有严格按照标准方式书写Given When Then，但实际上通过每个节点的上下文，我们就能了解到相应的这几方面的内容。

我觉得这种方式写Test Case的**优势**在于：
1. 有层次，一目了然
2. 也看得出AC的关联
3. 能知道哪些有测试实现
4. 能看得到测试结果
5. 任何角色都能看到MindMap，也都能了解相应的Story的状态。

对于Mind Map的**书写和分类**，目前我们是按照相对独立的Feature来分文件，Page-Feature-StoryID,方便找到需要的Mind Map。对于以后功能和页面过多的时候怎么分类（页面过多文件过多，功能多了后也必然会有不同页面的Feature的关联），还在思考中，逐步变化吧。

之前是每次修改后就上传到一个地方，要么重命名要么覆盖，后来就建议使用Svn之类的工具来进行版本管理以便于追踪修改。

与之对应，我们目前的**工作流程**是：
1. BA确定需求后会写出Mind Map的初稿；
2. QA和BA一起Review并得到最后的AC；
3. Kick Off Story时会按照Mind Map来过一遍
4. 在Developer进行开发的时候，会将已实现单元测试的方法名加在对应的AC之后并加上单元测试覆盖的图标；
5. QA能看到哪些AC被测试覆盖了，并着重未覆盖的AC也着重探索性，测试完成后会在对应的AC加上勾或者叉来标注结果。

有**欠缺**的地方有：
1. 多了一次对Story的描述，最开始是当做Story的补充，因此Mind Map能否及时根据Story更新也成了要关注的问题；
2. Mind Map是文档但不是可执行的文档。