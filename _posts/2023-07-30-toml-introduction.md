---
title:  "TOML Introduction"
description: "TOML 설정 파일 포맷의 기본 개념과 문법, Python에서 활용하는 방법을 알아봅니다. YAML, JSON과의 비교 및 실용적인 사용 예제 포함."
categories: programming tech
tags:
  - toml
  - yaml
  - python
  - configuration
  - 설정파일
  - json
  - file-format
  - pyproject
comments: true
---

TOML(Tom's Obvious Minimal Language)은 이름에서 알 수 있듯이 TOM이란 사람이
2013년에 만든 설정 파일 포맷(configuration file format)이다. TOML은
아래와 같은 목적으로 개발되었다.

1. 분명한 구문으로 사람이 쉽게 읽을 수 있어야 한다.
2. Python의 dictionary와 같은 hash table에 명확하게 맵핑되어야 한다.
3. 여러 언어의 자료구조(Data Structure)에 쉽게 파싱되어야 한다.

TOML 파일형식은 설정파일로 많이 쓰이고 있는 YAML 파일형식에 비견할 정도로 점진적으로
많이 쓰이고 있다. 이와 같은 여파로 Python 3.11에서는 TOML 라이브러리인 tomlib가
standard 라이브러리로 포함되었다. 필자가 주로 사용하는 poetry에서도 사용 중이며 또한
Rust의 Package Manager인 Cargo의 설정파일로 쓰이고 있다.

## 가독성

YAML과 TOML 추종자들은 이 가독성을 가지고 의견을 달리한다. 이 포스트에서 어느 것이 더
읽기 편한가에 대한 부분은 접어두기로 하고 독자들의 선택에 맡기도록 한다.

### TOML

```toml
title = "TOML Example"
 
[owner]
name = "Tom Preston-Werner"
dob = 1979-05-27T07:32:00-08:00
 
[database]
server = "192.168.1.1"
ports = [ 8001, 8001, 8002 ]
connection_max = 5000
enabled = true
 
[servers]
 
  [servers.alpha]
  ip = "10.0.0.1"
  dc = "eqdc10"
   
  [servers.beta]
  ip = "10.0.0.2"
  dc = "eqdc10"
   
[clients]
data = [ ["gamma", "delta"], [1, 2] ]
 
hosts = [
  "alpha",
  "omega"
]
```

### YAML

```yaml
title: YAML Example
 
owner:
  name: Tom Preston-Werner
  dob: 1979-05-27T07:32:00-08:00
 
database:
  server: 192.168.1.1
  ports: [ 8001, 8001, 8002 ]
  connection_max: 5000
  enabled: true
 
servers:
 
  alpha:
    ip: 10.0.0.1
    dc: eqdc10
   
  beta:
    ip: 10.0.0.2
    dc: eqdc10
 
clients:
  data: [ [gamma, delta], [1, 2] ]
   
  hosts:
    - alpha
    - omega
```

## YAML의 문제점

YAML은 좀더 복잡한 Syntax를 가지고 있다. 특히 Tags들은 다른 언어에서 다른 타입들을
추상화하기 위한 목적인데 이는 오히려 사람들을 헷갈리게 하고 있다.

**Note**: !!map Syntax는 python에서는 dictionary로 javascript에서는 object로 파싱된다.
{: .notice--warning}

이와 별개로 implict typing 문제를 가지고 있다.

**Note**: 기존에 string을 가지고 있는 밸류에 7을 넣으면 자동적으로 int로 파싱된다.
또한 yes/no도 Boolean True/False로 파싱될 수 있다.
{: .notice--warning}

이를 해결하기 위해  YAML 1.2에서 다른 schema들을 정의하여 해결해보려 했지만
Python의 YAML 라이브러리인 PyYAML이 YAML 1.1 기반으로 있게 되면서 이 문제를
해결하기 위해 PyYAML을 다시 쓴 libyaml, SnakeYAML과 같은 다양한 라이브러리들이
파생되면서 더욱 더 생태계를 혼잡하게 하였다.  
현재는 StrictYAML이 이와 같은 문제점들을 일부 해결하고
있다(StricYAML은 YAML 1.1 기반).  
  
이 포스트에서는 기존 YAML의 문제점들 중 하나로 알려진 **The Norway Problem**에
대해서만 알아보도록 하자. 노르웨이의 국가코드는 NO이며 기존 YAML은 이를
Boolean으로 파싱한다.

### The Norway Problem with YAML

#### test.yaml

```yaml
%YAML 1.1
---
countries:
  - GE
  - IE
  - FR
  - NO
```

#### yaml_test.py

```python
import yaml

with open('test.yaml', 'r') as file:
  countries = yaml.safe_load(file)
  print(countries)
```

- output

```sh
{'countries': ['GE', 'IE', 'FR', False]}
```

- 노르웨이의 국가코드가 False로 인식!

#### strict_yaml_test.py

```python
from path import Path
from strictyaml import load

with open('test.yaml', 'r') as file:
  countries = load(Path('./test.yaml').read_text()).data
  print(countries)
```

- output

```sh
{'countries': ['GE', 'IE', 'FR', 'NO']}
```

- 노르웨이의 국가코드를 NO로 인식

### The Norway Problem with TOML

#### test.toml

```toml
countries = [ "GE", "IE", "FR", "NO" ]
```

#### test_toml.py

```python
import tomllib

with open('test.toml', 'rb') as file:
  countries = tomllib.load(file)
  print(countries)
```

- output

```sh
{'countries': ['GE', 'IE', 'FR', 'NO']}
```