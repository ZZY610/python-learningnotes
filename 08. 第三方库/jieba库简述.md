# jieba库简述
jieba是一个第三方中文分词库。中文文本通过分词获得单个的词语。
jieba库的分词原理：利用一个中文词库，确定汉字之间的关联概率，汉字间概率大的组成词组，形成分词结果。除了分词，用户还可以添加自定义的词组。
```python
#// --run--
import jieba
s='今天是一个好日子'
print(jieba.lcut(s))
```

## jieba库分词有3种模式

* **精确模式**：就是把一段文本**精确地**切分成若干个中文单词，若干个中文单词之间经过组合，就精确地还原为之前的文本。其中不存在冗余单词。

* **全模式**：将一段文本中所有可能的词语都扫描出来，可能有一段文本它可以切分成不同的模式，或者有不同的角度来切分变成不同的词语，在全模式下，Jieba库会将各种不同的组合都挖掘出来。分词后的信息再组合起来会有**冗余**，不再是原来的文本。

* **搜索引擎模式**：在精确模式基础上，对发现的那些长的词语，我们会对它再次切分，进而适合搜索引擎对短词语的索引和搜索。也有冗余。
## 常用函数
| 函数 | 描述 |
| -- | -- |
| jieba.cut(s) | 精确模式，返回一个可迭代数据类型 |
| jieba.cut(cut_all=true) | 全模式，输出文本s中所有可能的词语 |
| jieba.cut_for_search(s) | 搜索引擎模式 |
| jieba.lcut(s) | 精确模式，返回列表类型 |
| jieba.lcut(s,cut_all=true) | 全模式，返回列表类型 |
| jieba.lcut_for_search(s) | 搜索引擎模式，返回列表 |
| jieba.add_word(w) | 增加新词 |

统计文本中的词频，返回词频对构成的字典。
```python
#--run--
import jieba
s="如今男人们在学着将自己的羽箭上黏上羽毛，学着用更好的办法拉弓，而女人们则传递着那几个抢来的陶罐，想知道为什么会这么圆。"
def fun1(s):  # 这个函数用来统计文本中的高频词，返回一个“词-频”对的字典
    counts = {}
    x = jieba.lcut(s)
    for word in x:
        counts[word] = counts.get(word, 0) + 1
    items = list(counts.items())
    items.sort(key=lambda x: x[1], reverse=True)
    return dict(items[:150])
if __name__ == '__main__':
    print(fun1(s))
```
