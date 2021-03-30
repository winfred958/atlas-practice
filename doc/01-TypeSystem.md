# Type System

- ## atlas 重要概念
    - ### [Type System](http://atlas.apache.org/#/TypeSystem)
        - ```text
          atlas 允许用户对其想要管理的 metadata 自定义 model, 这个 model 由 Types 组成.
          Types 的 instance 被称为 entities, 表示实际管理的 metadata 对象.
          该 Type System 允许用户定义和管理 types 和 entities.
          被Atlas 管理的所有的 metadata objects都是开箱即用的, 使用 Types 建立model 表示 entities. 
          要在 atlas 中存储 新的 metadata Type, 需要理解 Type System 组件的概念.
          ```
    - ### Types
        - **Type** 定义如何存储和访问特定的metadata.
        - 1个Type有1个或多个 **attributes** 用来定义 metadata 对象.
        - 有 OOP 开发背景的用户, 会识别出 "Class" 和 "table schema" 有相似之处.
        - 一个Type的例子, atlas 典型的例子定义 hive table
          ```yaml
          Name:         hive_table
          TypeCategory: Entity
          SuperTypes:   DataSet
          Attributes:
            name:             string
            db:               hive_db
            owner:            string
            createTime:       date
            lastAccessTime:   date
            comment:          string
            retention:        int
            sd:               hive_storagedesc
            partitionKeys:    array<hive_column>
            aliases:          array<string>
            columns:          array<hive_column>
            parameters:       map<string>
            viewOriginalText: string
            viewExpandedText: string
            tableType:        string
            temporary:        boolean
          ```
        - 从上面的例子可以看出以下几点
            - 1 个 Type 在 Atlas中用 name 作为唯一标识.
            - Type 有元类型(基本(原始)类型, 枚举, 集合, 复合类型)
                - Primitive 类型: boolean, byte, short, int, long, float, double, biginteger, bigdecimal, string, date
                - Enum type
                - Collection type: array, map
                - Composite type: Entity, Struct, Classification, Relationship
                    - Entity & Classification 可以从其他 Type 继承, 被称为 **supertype**. 因此包含supertype的的attribute. 这种设计方式,
                      允许用户定义一组通用的 attribute 跨多个有关联的 types. 类似面向对象中父子类的关系.
            - Type attribute集合由'基本类型','Entity','Struct','Classification','Relationship' 组成. 每个 attribute 有一个 name
              和其他相关属性.
            - 1个attribute可以被引用, 使用表达式 {type_name}.{attribute_name}, attribute 本身是使用 atlas metatypes(元类型, 基本类型) 定义的.
            - attribute 中的 类型引用, {type_name}.{attribute_name}. 需要注意的是,...

    - ### Entities
        -  ```text
           1个 entity 是个特殊的值, 是type 的 instance, 从而代表1个真实世界中的 metadata object.
           参照OOP 语言, instance 是某一个Class的实例Object.
           ```
        - 下面是一个 Hive Table的例子, 1个被称为'customers', 这个 table 是1个 hive_table(type) 的  'entity'.
            - ```yaml
              guid:     "9ba387dd-fa76-429c-b791-ffc338d3c91f"
                typeName: "hive_table"
                status:   "ACTIVE"
                values:
                name:             “customers”
                db:               { "guid": "b42c6cfc-c1e7-42fd-a9e6-890e0adf33bc",
                    "typeName": "hive_db"
                    }
                owner:            “admin”
                createTime:       1490761686029
                updateTime:       1516298102877
                comment:          null
                retention:        0
                sd:               { "guid": "ff58025f-6854-4195-9f75-3a3058dd8dcf",
                    "typeName":
                    "hive_storagedesc"
                    }
                partitionKeys:    null
                aliases:          null
                columns:          [ { "guid": "65e2204f-6a23-4130-934a-9679af6a211f",
                    "typeName": "hive_column" },
                    { "guid": "d726de70-faca-46fb-9c99-cf04f6b579a6",
                    "typeName": "hive_column" },
                    ...
                    ]
                parameters:       { "transient_lastDdlTime": "1466403208"}
                viewOriginalText: null
                viewExpandedText: null
                tableType:        “MANAGED_TABLE”
                temporary:        false
              ```
        - 从上面例子可以看出
            - 每个 type 的实例 entity 都有一个唯一的 id(unique identifier), GUID, 由 atlas server 产生.
            - entity 来自定的type, type 提供了 entity 的定义
                - 这个例子中, 'customers' table is a 'hive_table'
            - entity 的 value 是 hive_table Type 定义的所有 attributes.
            - attribute 的 value 将根据 attribute 的 datatype 决定. Entity- type attribute 的 value 为 AtlasObjectId.

    - ### Attributes
        - ```text
          attribute 被定义为 inside type 像 Entity, Struct, Classification 和 Relationship.
          但是我们简单的理解, attribute 有 1个name和1个metatype value. 无论怎样, attribute in Atlas 有许多 properties 
          这些属性定义了更多与 Type System相关的概念
          ```
        - 1个 attribute 具有的 properties:
            - ```yaml
              name:        string,
              typeName:    string,
              isOptional:  boolean,
              isIndexable: boolean,
              isUnique:    boolean,
              cardinality: enum
              ```
        - 这些属性有以下含义:
            - name
                - attribute 的 name
            - dataTypeName
                - metatype (native, collection or composite)
            - isComposite: 符合类型,
            - isIndexable:
                - 是否使用这个attribute建立索引, 以便使用这个值进程检索.
            - isUnique:
                - 这个 flag 和索引有关, 如果指定为 unique, 意味着特殊的所有将会被JanusGraph创建, 允许使用 equals 查找.
                -
            - multiplicity