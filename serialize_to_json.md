# SQLAlchemy序列化的几种方式



- [SQLAlchemy-serializer](https://pypi.org/project/SQLAlchemy-serializer/)

  ```python
  item = SomeModel.query.filter(...).one()
  result = item.to_dict()
  """
  1. 支持外键嵌套查询
  2. to_dict接受rules和only参数客制化序列值
  3. 但貌似不支持select_from join后table对象（待确认）
  """
  # 4. 一对多，序列化多的一方时会报RecursionError，指定嵌套层级
  many_obj.to_dict(rules=("-parent.children",))
  # 5. project(1-多)app(1-多)datasource，三层外键，datasource序列化出project的信息，如下
  datasource.to_dict(rules=("-app.datasources", "-app.project.apps"))
  ```

  

- 简单序列化

  ```python
  class User:
     def as_dict(self):
        # best
         return {c.name: getattr(self, c.name) for c in self.__table__.columns}
     
     def to_json(self):
        # join后的对象无法序列化
        _dict = self.__dict__
        if "_sa_instance_state" in _dict:
          del _dict["_sa_instance_state"]
          return _dict
  ```

  