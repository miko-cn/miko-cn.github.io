# Footnotes - 脚注

脚注是添加补充或附加信息到特定单词、短语或句子而不打断文档流程的绝佳方式[^1]。MkDocs材料提供了定义、引用和渲染脚注的能力[^2]。

!!! note "脚注注意事项"
    1. 不同编号的脚注的内容可以放置在不同的位置，但要确保同一标号的脚注只有一个；
    2. 脚注内容使用的语法`[^i]:`中，中括号和冒号都是英文符号。

```md
普通的段落[^1],另外一个段落[^2]。

[^1]: 脚注1
[^2]: 脚注2
```

[^1]: 这是第一个脚注。
[^2]: 这是第二个脚注。
