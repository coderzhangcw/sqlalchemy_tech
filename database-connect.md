# 与数据库建立连接

```python
from sqlalchemy import create_engine
# sqlite，echo=True打印sql
engine = create_engine('sqlite:///test.sqlite3', echo=True)
```

```python
# postgresql driver一般使用psycopg2就可以
# postgresql+psycopg2://user:password@host:port/dbname[?key=value&key=value...]
# default，postgresql默认DBAPI是psycopg2
engine = create_engine("postgresql://scott:tiger@localhost/mydatabase")

# psycopg2
engine = create_engine("postgresql+psycopg2://scott:tiger@localhost/mydatabase")

# pg8000
engine = create_engine("postgresql+pg8000://scott:tiger@localhost/mydatabase")
```

```python
# mysql driver可用pymysql
engine = create_engine("mysql+pymysql://user:pass@some_mariadb/dbname?charset=utf8mb4")

# default，MySQL dialect uses mysqlclient as the default DBAPI.
engine = create_engine("mysql://scott:tiger@localhost/foo")

# mysqlclient (a maintained fork of MySQL-Python)
engine = create_engine("mysql+mysqldb://scott:tiger@localhost/foo")

# PyMySQL
engine = create_engine("mysql+pymysql://scott:tiger@localhost/foo")
```

```python
# 还可以自动生成数据库链接URL
from sqlalchemy import URL

url_object = URL.create(
    "postgresql+pg8000",
    username="dbuser",
    password="kx@jj5/g",  # plain (unescaped) text
    host="pghost10",
    database="appdb",
)

from sqlalchemy import create_engine

engine = create_engine(url_object)
```



### 建立数据库engine后，建立会话

```python
from sqlalchemy.orm import Session
session = Session(engine)
# 或者
from sqlalchemy.orm import sessionmaker
DBSession = sessionmaker(bind=engine)
```

