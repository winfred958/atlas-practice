# Glossary

```text
glossary: 词汇表, 字典表.
term: 词汇(术语), 有助于抽象底层存储相关的技术术语, 允许用户映射到熟悉的词汇, 作为映射.
```

- glossary 为 business user 提供了特殊的词汇 (term).
    - 允许 terms(words) 相互关联.
    - 将被用来分类在不同的上下文中.
- 这些 terms 能被映射到 Database, table, columns 等其他资产.
- **用处**:
    - 有助于抽象存储相关的技术术语, 允许用户映射到熟悉的词汇.

## Use cases

- 允许定义自然术语, 丰富(屏蔽)技术和商业词汇.
- 允许在语义上关联其他 term
- 允许映射资产(DB, table, column)到 glossary terms.
- 允许按 category 组织 terms, 这个增加term的上下文表达能力.
- 允许按分类层次结构排列, 以表示更宽泛或精细的范围.
- 分开管理 glossary term 和 metadata.

## Glossary term

- A term 在 apache atlas 中必须有唯一的限定名; 可以有相同的term name, 但是相同term name 不能属于同一个 glossary.
- Term 可以包含 空格, 下划线, 但是不能有 "." or "@".
- A term 只能属于一个 glossary. 删除一个 Glossary , 将会删除glossary下所有的term.
- A term 可以属于0或多个 category, 可以定义term在上下文的作用域.
- A term 可以被映射/链接到0个或多个 entity.
- A term 可以使用 classifications(tags)进行分类, 并且将相同的 classification 应用到 entities, 则 term 也会归属于这个 entities.