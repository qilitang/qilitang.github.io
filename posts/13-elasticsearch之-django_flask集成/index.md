# 

## 一 elasticsearch-dsl

```python
#安装： pip3 install elasticsearch-dsl
#示例
from datetime import datetime
from elasticsearch_dsl import Document, Date, Nested, Boolean, \
    analyzer, InnerDoc, Completion, Keyword, Text

html_strip = analyzer('html_strip',
    tokenizer="standard",
    filter=["standard", "lowercase", "stop", "snowball"],
    char_filter=["html_strip"]
)

class Comment(InnerDoc):
    author = Text(fields={'raw': Keyword()})
    content = Text(analyzer='snowball')
    created_at = Date()

    def age(self):
        return datetime.now() - self.created_at

class Post(Document):
    title = Text()
    title_suggest = Completion()
    created_at = Date()
    published = Boolean()
    category = Text(
        analyzer=html_strip,
        fields={'raw': Keyword()}
    )

    comments = Nested(Comment)

    class Index:
        name = 'blog'

    def add_comment(self, author, content):
        self.comments.append(
          Comment(author=author, content=content, created_at=datetime.now()))

    def save(self, ** kwargs):
        self.created_at = datetime.now()
        return super().save(** kwargs)
```

## 二 django集成

```python
from datetime import datetime
from elasticsearch_dsl import Document, Date, Nested, Boolean,analyzer, InnerDoc, Completion, Keyword, Text,Integer

from elasticsearch_dsl.connections import connections

connections.create_connection(hosts=["localhost"])


class Article(Document):
    title = Text(analyzer='ik_max_word', search_analyzer="ik_max_word", fields={'title': Keyword()})
    author = Text()

    class Index:
        name = 'myindex'

    def save(self, ** kwargs):
        return super(Article, self).save(** kwargs)


if __name__ == '__main__':
    # Article.init()  # 创建映射
    # 保存数据
    # article = Article()
    # article.title = "测试测试"
    # article.save()  # 数据就保存了

    #查询数据
    # s=Article.search()
    # s = s.filter('match', title="测试")
    #
    # results = s.execute()
    # print(results)

    #删除数据
    # s = Article.search()
    # s = s.filter('match', title="测试").delete()

    #修改数据
    # s = Article().search()
    # s = s.filter('match', title="测试")
    # results = s.execute()
    # print(results[0])
    # results[0].title="xxx"
    # results[0].save()
```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/13-elasticsearch%E4%B9%8B-django_flask%E9%9B%86%E6%88%90/  

