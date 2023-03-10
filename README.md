## 详细文档

有关该项目的详细设计及效果测试，见release

https://github.com/chuan-boss/library/releases/tag/v1.0

## 使用说明

### 运行系统

Windows/Linux

### 相关依赖(配置)

* 语言：python,vue,mysql

* 工具：npm

* 依赖包：

  * python : 

    ```python
    asgiref      3.5.2
    cffi         1.15.1
    cryptography 38.0.4
    Django       4.1.4
    pip          21.3.1
    pycparser    2.21
    PyMySQL      1.0.2
    setuptools   60.2.0
    sqlparse     0.4.3
    tzdata       2022.7
    wheel        0.37.1
    
    ```

  * vue : 

    ```vue
    切换到Web目录运行：
    		npm install
    (依赖包过多，不一一列出)
    ```

### 运行

#### 	数据库

```
//切换到DATABASE文件夹
cd
```

更改init.py中的密码为自己root账号的密码

![image-20221208200942505](.\MD_IMG\image-20221208200942505.png)

```
python Run.py
```


#### 	前端

``` 
//切换到相应目录
cd /Web
//安装相关依赖
npm install
//运行
npm run serve

```

#### 	后端

```
//切换到Server文件夹
cd 

python manage.py migrage
python manage.py runserver
```


## 数据库逻辑结构

- ### 用户

    | ID   | Name | Password | Is_admin | Email |
    | ---- | ---- | -------- | -------- | ----- |

    在该表中，id是主键

- ### 书

    总表：

    | ISBN | title | author | press | price | score | num_of_scorers | num_of_comments |
    | ---- | ----- | ------ | ----- | ----- | ----- | -------------- | --------------- |
 
    在该表中，ISBN是主键

    在插入书籍信息后，由于评分，评论还未发布，所以score，num_of_scorers，num_of_comments初始状态为空。在有用户进行评论后，通过设置触发器，在评分表中计算以及在评论表中按照ISBN筛选统计目数即可实时更新这三条信息。

- ### 书籍-评论表

    | ISBN | user_id | 时间戳 | remark（评论） |
    | ---- | ------- | ------ | -------------- |

    主键 ： (ISBN, user_id, 时间戳)

- ### 书籍-评分表

    | ISBN | user_id | score |
    | ---- | ------- | ----- |

    主键：(ISBN, user_id)

    

- #### 视图：

    - ##### 书籍基本信息表：

      | ISBN | title | author | press |
      | ---- | ----- | ------ | ----- |

      在该表中，ISBN是主键

    - ##### 书籍-出版社表（一本书可以有多个出版社，但不同出版社发行的同一本书的ISBN也是不同的）

      | ISBN | press |
      | ---- | ----- |

      在该表中，ISBN是主键

    - ##### 书籍-定价表：

      | ISBN | price |
      | ---- | ----- |

      主键：ISBN

    - ##### 书籍-评分(总)表：

      | ISBN | score |
      | ---- | ----- |

      主键：ISBN
  
    - ##### 书籍-作者表
  
      | ISBN | Author |
      | ---- | ------ |




## 功能设计（以页面的形式呈现）

* **登录注册页面**

  实现的功能：登录/注册

  需要用到的表：用户表

* **个人信息页**

  实现的功能：个人信息展示、个人信息修改（是否是管理员不能修改）

  需要用到的表：用户表

* **书的浏览界面**

  实现的功能：

  * 查看书籍
    * 分类查看
      * 价格区间
      * 种类
      * 作者
      * 出版社
      * 评分区间
      * 联合筛查
    * 对于每个书籍可以查看详情
      * 价格、作者......
      * 查看评论
  * 添加评论
  * 管理员特殊功能
    * 删除评论
    * 删除/上架书籍

  需要用到的表：用户表（查看权限），书籍表，评论表

