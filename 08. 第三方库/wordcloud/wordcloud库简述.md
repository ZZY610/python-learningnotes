# wordcloud库简述
wordcloud库把词云当作一个WordCloud对象。wordcloud.WordCloud()代表一个文本对应的词云，可以根据文本中词语频率等参数绘制词云。
绘制词云的形状，尺寸和颜色都可以设定。

## wordcloud.WordCloud()方法
返回 `wordcloud` 类。

| 参数 | 数据据型 | 说明 |
| -- | -- | -- |
|font_path | string | 使用字体的路径，Windows系统下可以在C:\Windows\Fonts中查找本机已安装的字体。|
|**`width,height`**| int (default=400)，int (default=200)| 词云的宽高。|
|prefer_horizontal | float (default=0.90) |水平拟合单词占比。与垂直拟合单词占比默认为0.1|
|**`mask`** |  nd-array or None (default=None) |词云的形状；我们可以找一张素材图，用PIL读取并转换成numpy数组然后传入这个参数。白色区域会被直接忽略。|
|contour_width | float (default=0) |词云的轮廓|
|contour_color | color value (default="black") |轮廓的颜色|
|scale | float (default=1) |对词云按比例缩放。如果参数过小会显得很模糊。大的词云会更精细。|
|min_font_size | int (default=4)  |显示的字体最小值|
|max_font_size | int or None (default=None)  |显示字体的最大值|
|font_step | int (default=1)  |字体步长，或者说间距；若>1加快运算但拟合效果下降。|
|max_words | number (default=200) |词云显示的单词数最大值|
|stopwords | set of strings or None  |将被屏蔽的单词。 如果为None，将使用内置STOPWORDS列表。 如果使用generate_from_frequencies则忽略。  |
|background_color |  color value (default=”black”) |背景颜色|
|mode | string (default="RGB") |当mode为“RGBA”且background_color为None时，将生成透明背景。  |
|relative_scaling | float (default=auto) |词频对字体大小的权重。 当relative_scaling=0时，只考虑单词顺序。 如果relative_scaling=1，那么频率加倍的单词大小也会加倍。 如果你想考虑单词频率，而不仅仅是它们的排名，relative_scaling在0.5左右通常看起来不错。 如果'auto'，它将被设置为0.5，除非repeat为true，在这种情况下，它将被设置为0。|
|color_func | callable, default=None |为每个单词返回一个PIL颜色数据|
|regexp |  string or None (optional) |使用正则表达式分割文本|
|collocations |  bool, default=True  |是否包含两个词的搭配|
|colormap | string or matplotlib colormap, default="viridis" |给单词随机分配颜色|
|normalize_plurals | bool, default=True |是否删除单词末尾的s并添加到不带s的词里去，用于统计英文文本|
|repeat | bool, default=False |是否重复单词，直到达到max_words或min_font_size。  |
|min_word_length | int, default=0 |一个单词必须包含的最小字母数。  |

## wordcloud类的方法
绘制词云三步：
* 配置对象参数
* 加载词云文本
* 输出词云文件

|方法|参数|返回值|说明|
| -- | -- | -- | -- |
|generate(); generate_from_text() |text:str|<class 'wordcloud.wordcloud.WordCloud'>	|输入的“文本”应该是自然的文本。 如果传递一个排序的单词列表，单词将在输出中出现两次。 要删除这个重复，设置' '  collocations=False ' '。  |
|generate_from_frequencies()|frequencies, max_font_size=None;frequencies : dict from string to float; max_font_size : int;Use this font-size instead of self.max_font_size|同上|根据词频生成词云。|
| fit_words() | frequencies | 	同上	 | 根据词频生成词云 |
| to_file() | filename	 | 同上	 | 保存词云图片 |
| to_array() | 无	 | ndarray	 | 将词云转换为numpy数组 |