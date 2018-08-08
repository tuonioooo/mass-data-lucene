# 分词器

## 什么是Tokenizer-分词

分词器的工作就是分解文本流成词\(tokens\).在这个文本中,每一个token都是这些字符的一个子序列.一个分析器\(analyzer\)必须知道它所配置的字段,但是tokenizer不需要,分词器\(tokenizer\)从一个字符流\(reader\)读取数据,生成一个Token对象\(TokenStream\)的序列.

输入流中的一些字符可能会被丢弃,如空格和一些分隔符;也可能会被添加或者替换,如别名映射和缩写.一个token包含多种元数据除了它的原始文本值,如字段中词\(token\)出现的位置.因为分词器从输入文本中发散之后生成词\(tokens\),你是不能假定token的文本和字段中出现的文本相同的.在原始的文本中很有可能超过一个的token拥有相同的位置或者关联相同的偏移量\(offset\).如果你使用token元数据做高亮时,请注意这一点儿.

```
<fieldType name="text" class="solr.TextField">
    <analyzer>
        <tokenizer class="solr.StandardTokenizerFactory" />
    </analyzer>
</fieldType>
```

这里边tokenizer元素的class的值并不是实际的值,而是一个实现了org.apache.solr.analysis.TokenizerFactory接口的类.这个工厂类被调用用来创建所需要的新的tokenizer实例.源自org.apache.lucene.analysis.TokenStream,工厂创建的对象显示了它们产生的tokens序列.如果tokenizer生成的token正是它所需要的,那么它也许就是analyzer的唯一组件.否则,分词器的输出的tokens将作为管道中第一个过滤器的输入.

TypeTokenFilterFactory可用于创建一个TypeTokenFilter,这个对象过滤tokens基于它们的TypeAttribute的.可以在factory.getStopTypes中设置.

## CharFilter vs  TokenFilter

这里有好几对的CharFilters和tokenFilters是有关联\(MappingCharFilter和ASCIIFoldingFilter\)或者是几乎相同\(PatternReplaceCharFilterFactory和PatternReplaceFilterFactory\)的功能.通常不好区分哪一个才是最好的选择.

使用哪个过滤器很大程度上依赖于你使用的是哪个分词器\(tokenizer\),你是否需要预处理字符流.

举例来说,假设你有一个StandardTokenizer的分词器,并且你很想知道它整体上是如何工作的,你想要自定义了一些指定的字符的行为表现.你需要修改你的规则,重新编译你得分词器\(tokenizer\).但是在分词前使用一个charFilter简单映射一些字符会使它变的更加简单.

## lucene分词器Analyzer

**参考**

Lucene 3.0 原理与代码分析完整版

1.26 Lucene学习总结之十：Lucene的分词器Analyzer

链接: [https://pan.baidu.com/s/1kN5sUUf07XcO2Nv1GzxemQ](https://pan.baidu.com/s/1kN5sUUf07XcO2Nv1GzxemQ) 密码: haxp

