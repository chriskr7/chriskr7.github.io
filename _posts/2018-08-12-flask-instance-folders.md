---
title:  "Flask Instance Folders"
categories: programming
tags:
  - flask
  - "instance folders"
  - python
comments: true
---
OS는 각각의 프로세스들의 working directory들을 알고 있다.
하지만 Web-Application에서는 하나의 프로세스안에 여러 개의 Application들을
가질 가능성이 있다. Flask에서는 app.root_path와 os.path를 이용하여 원하는
파일을 찾을 수 있지만 Application이 package일 경우
root_path는 package의 contents를 바라보기 때문에 올바르게 작동하지 않는다.

이런 이유로 Flask 0.8에서 **flask.instance_path**라는 새로운 속성를 추가하였다.
이 속성은 Instance Folders라는 새로운 개념을 이용한다.
Instance Folders는 version control안에 있지 않고 Deployment Specific이다.
그러므로 이 폴더가 config/설정 파일들을 넣기에는 최적의 장소이다.

```python
app = Flask(__name__, instance_path='/path/to/instance/folder')
```

여기서 instance_path에 지정하는 경로는 절대 경로이다.
instance_path가 주어지지 않았다면 아래의 default 폴더가 사용된다.

* Uninstalled module

```python
/app.py
/instance
```

* Uninstalled package

```python
/myapp
  /__init__.py
/instance
```

* Installed module or package

```python
$PREFIX/lib/python2.X/site-packages/myapp
$PREFIX/var/myapp-instance
```

$PREFIX는 Python 설치 폴더이다.

Flask에서는 보통 config 파일에서 config object를 load하므로 instance 폴더의 위치의 상관없이 instance 폴더안의 config 파일을 load할 수 있게 있다. (default는 application root 폴더를 보고 있다.)
예를 들면 Uninstalled package가 아래와 같이 있다고 할 때

```python
/myapp
  /__init__.py
/instance
```

**1번**

```python
app = Flask(__name__)
app.config.from_pyfile('application.cfg', silent=True)
```

**2번**

```python
app = Flask(__name__, instance_relative_config=True)
app.config.from_pyfile('application.cfg', silent=True)
```

위의 1번에서는 default가 application root 폴더이므로 /myapp 폴더안에서
application.cfg를 찾을 것이며, 2번에서는 instance_relative_config를 True로
설정하므로 instance 폴더가 어디에 있든지
instance 폴더안에서 application.cfg에서 찾을 것이다.
**silent=True**는 해당 폴더에 application.cfg파일이 없어도
complain하지 말고 조용히 있으라는 설정이다.

instance folder의 경로는 **Flask.intance_path**를 통해 알 수 있다.
Flask는 또한 instance folder에서 파일을 열기 위한
shortcut인 **Flask.open_instance_resource()** method를 제공하고 있다.

```python
filename = os.path.join(app.instance_path, 'application.cfg')
with open(filename) as f:
  config = f.read()

# or via open_instance_resource
with app.open_instance_resource('applicatoin.cfg') as f:
  config = f.read()
```

---

### Reference

+ [Configuration Handling — Flask 1.0.2 documentation](http://flask.pocoo.org/docs/1.0/config/)
