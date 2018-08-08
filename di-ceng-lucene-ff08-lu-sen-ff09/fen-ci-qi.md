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

