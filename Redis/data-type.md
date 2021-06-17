# Strings
redis에서 사용하는 가장 일반적인 형태이다. 이 타입은 binary safe한데, JPEG 이미지 등 어떤 형태의 데이터도 저장 가능하다는 것을 의미한다. 최대 512Mb의 길이를 저장할 수 있다. strings 타입으로는 아래에 있는 것들을 할 수 있다.
APPEND 명령어를 사용하여 기존 문자열과 이어붙이는 것이 가능하다.
INCR family(incrby, decrby)를 사용하여를 atomic한 카운터를 구현할 수 있다.
GETRANGE나 SETRANGE를 사용하여 문자열 중 아무 위치로 접근할 수 있다.

# Lists
Lists는 strings의 입력 순서로 정렬된 단순한 배열이다. List의 맨 왼쪽(head)나 맨 오른쪽(tail)에 추가하는 것이 가능하다. LPUSH는 왼쪽으로 추가하는 것이고 RPUSH는 오른쪽으로 추가하는 것이다.
LPUSH mylist a # now the list is "a" 
LPUSH mylist b # now the list is "b","a" 
RPUSH mylist c # now the list is "b","a","c" (RPUSH was used this time)

하나의 리스트에 입력 가능한 최대 개수는 $2^{23} - 1$ 개다.
가장 주요 기능은 head나 tail로 입력하는 것이나 삭제하는 것의 시간 복잡도(time complexity)가 상수시간 내로 보장이 된다는 점이다. 만약 리스트 내의 head 나 tail 에 가까운 요소에 접근한다면 굉장히 빠른 성능을 보일 수 있지만, 아주 많은 elements를 가지는 리스트 내에서 가운데에 위치한 요소에 접근한다면 O(N)의 연산을 하게 되면서 속도가 느려질 수 있다.
리스트로는 아래와 같은 것들을 할 수 있다.
소셜 네트워크의 타임라인을 모델링할 때, LPUSH를 사용하여 새로운 element를 유저의 타임라인에 추가할 수 있고, LRANGE를 이용하여 최근에 추가된 item들의 일부만을 가져올 수 있다.
리스트는 메시지 전달을 위하여 사용될 수도 있다.
리스트로 할 수 있는 일은 BLPOP등 정말 많다.

# Sets
Sets는 순서 정렬이 보장되지 않는 Strings의 collection이다. 추가, 삭제, 존재 여부 판단을 하는 명령어는 O(1)내로 실행가능하다. Sets도 일반적인 set과 유사하게 같은 이름으로 된 멤버를 허용하지 않는다. 만약 같은 이름의 멤버를 추가하는 것을 시도하면 동일한 copy가 생긴다. 중복을 확인한 후 추가 명령어를 입력해야 하는 것이 아니다. Sets에 재미있는 점은 많은 서버사이드 명령어를 지원한다는 것인데, set을 활용하여 합집합, 차집합, 여집합등을 빠른 시간 내에 연산할 수 있다. list와 마찬가지로 입력 가능한 최대 개수는 $2^{23} - 1$ 개다.
unique한 값을 트래킹할 때 사용하기 좋다. 특정 블로그에 방문한 모든 unique한 IP 주소를 알고 싶을 때, SADD를 사용하면 중복되는 IP 주소를 입력하는 일이 없을 것이다.
set은 관계를 표현하기에도 적합하다. 태깅 시스템을 구현할 수도 있다. 하나의 객체에 해당하는 태그들을 set으로 저장하면 된다. 동시에 세 태그를 가지고 있는 객체의 모든 아이디를 원한다면 SINTER를 사용할 수 있다.

# Hashes
Hashes는 string field 와 string value 사이의 매핑하므로, 이름이나 나이 혹은 성별 등을 가지는 하나의 유저처럼 어떠한 객체를 표현하기에 적합하다.
HMSET user:1000 username antirez password P1pp0 age 34 
HGETALL user:1000 HSET user:1000 password 12345 
HGETALL user:1000
적은 필드를 가지는 해시는 아주 작은 공간을 차지하므로 수백만의 객체를 아주 작은 인스턴스에 저장할 수 있다.
하나의 해시는 40억개 이상의 field-value쌍을 저장할 수 있다.

# Sorted sets
Sorted Sets는 Sets와 비슷하게 중복되는 값이 들어가지 않는다. 차이점은 sorted set의 모든 요소들은 score를 가지고 이 score값에 의하여 정렬이 된다는 것이다. member 값은 중복이 될 수 없지만 score값은 순서를 정렬하기 위한 값이므로 중복이 가능한 값이다.
sorted set은 위치가 중간에 위치하더라도 접근하는 속도가 빠르다. 그러므로 어떤 값에도 항상 빠른 접근이 필요할 때 사용하기 좋다.
큰 규모의 온라인 게임에서 리더보드를 만들 때, ZADD를 이용하여 점수를 업데이트시켜준다면 ZRANGE를 이용하여 쉽게 상위 유저를 뽑아낼 수 있고, ZRANK를 이용하여 특정 유저에게 랭킹을 보여줄 수 있다. 근데 이걸 빠르게.
sorted set은 레디스에 저장된 값을 인덱싱할 때에도 종종 쓰인다. 만약 유저를 나타내는 많은 해시를 갖고 있다고 해보자. 그러면 sorted set의 스코어에 유저의 나이(정수)를 넣고 아이디를 값으로 넣는다고 하면, ZRANGEEBYSCORE를 활용하여 나이에 따라서 알고 싶은 정보를 얻을 수도 있다.
sorted set은 아마 가장 많은 기능을 하는 데이터 타입일 것 같다. 그러니 전체 명령어를 찬찬히 읽어보면서 레디스로 할 수 있는 것들을 발견하면 좋을 것 같다!
추가로 레디스를 활용하여 갑작스럽게 주문이 몰릴 때 sorted set을 활용하여 해결하는 것 같기도 하다 

# Bitmaps and HyperLogLogs
레디스는 Bitmaps과 HyperLogLogs도 지원하는데, 이건 사실 String base type에 기반한 데이터타입이다.
 
더 자세히 알아보기
https://redis.io/topics/data-types-intro
출처
https://redis.io/topics/data-types
