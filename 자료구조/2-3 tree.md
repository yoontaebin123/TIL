# 2-3 Tree
+ 2-3 Tree는 트리의 높이가 균형을 이루며 내부노드의 차수가 2 또는 3인 균형 탐색트리이다.

## 2-3 Tree 조건
+ 2-3 Tree에는 Internal Node와 External Node의 개념이 존재한다.
    + **Internal Node(내부노드)** : Key가 들어있는 내부 노드
    + **External Node(외부노드)** : 데이터가 들어있지 않은 노드로써 Internal Node의 Leaf Node의 자식으로 존재하는 가상의 노드
    ![2-3 tree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FduEu7W%2FbtrEtnu6tFI%2Fk4pwLOqB38NDYoCHWnIdoK%2Fimg.png)
+ 중복된 Key는 허용하지 않는다.
+ 2-3 Tree에서 각각의 내부 노드는 2-Node 이거나 3-Node 이다.
    + **2-Node** : 1개의 Key를 가지며, 2개의 자식 노드를 가진다.
    + **3-Node** : 2개의 Key를 가지며, 3개의 자식 노드를 가진다.
+ leftChild와 middleChild를 각각 2-node의 왼쪽 자식 및 중간 자식이라고 하고 leftKey를 해당 노드의 key라 하겠습니다.
    + 루트가 leftChild인 모든 서브트리의 key 값은 leftKey보다 작고, 루트가 middleChild인 모든 서브트리의 key 값은 leftKey보다 크다.
+ leftChild와 middleChild, rightChild를 각각 3-node의 왼쪽, 중간 및 오른쪽 자식이라 하고 leftKey, rightKey를 해당 노드의 key라 하겠습니다.
+ rightKey는 leftKey보다 크다.
    + 루트가 leftChild인 모든 서브트리의 key 값은 leftKey보다 작고, 루트가 middleChild인 모든 서브트리의 key 값은 leftKey보다 크며 rightKey보다 작다.
    루트가 rightChild인 모든 서브트리의 key 값은 rightKey보다 크다.
![2-3 tree](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMQNl9%2FbtrEnvgZupf%2FsYJoLnd4HyTWoe0m7ElQ1k%2Fimg.png)

## 예시
+ Root가 2-Node인 2-3 Tree
 ![2-node](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbpsSVP%2FbtrEqi1J2Ow%2Fvhc23gS5S2OrS9gpasqmHk%2Fimg.png)
+ Root가 3-Node인 2-3 Tree
  ![3-node](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fwd0Ss%2FbtrEnKroqUc%2Fp3zhRxXpxSclxfWKc3wQAK%2Fimg.png)


## 검색
+ 찾으려는 값을 Root 노드의 leftKey, rightKey와 비교한다.
    + leftKey보다 작은 경우 - leftSubtree를 탐색한다.
    + leftKey보다 크고, rightKey보다 작은 경우 - middleSubtree를 탐색한다.
    + rightKey보다 큰 경우 - rightSubtree를 탐색한다.

## 추가
+ 우선 노드를 추가할 때, 기본적인 규칙은 반드시 Leaf Node에 데이터를 삽입한다.
+ 부모노드에 빈 공간이 있다고 해서 부모 노드에 삽입하지 않는다. 반드시 데이터는 Leaf Node에 삽입한다.
    1. 값이 추가될 위치를 찾는다.
    2. 추가될 Leaf Node가 용량을 초과하지 않았다면 오름차순으로 데이터를 삽입한다. 만약 용량을 초과하였다면 Split(분할)을 수행해야 한다.


## Split(분할)
+ 추가될 데이터를 포함하여 중앙값을 결정한다.
+ 중앙값을 부모 노드에 추가시키며 중앙값보다 작은 값들을 왼쪽 서브트리, 중앙값보다 큰 값들은 오른쪽 서브트리가 된다.
+ 부모 노드가 없다면 새로 생성하며, 부모 노드에서도 용량이 가득찼다면 똑같이 Split 하는 과정을 반복한다.


## 삭제
+ 우선 삭제의 경우 Rotation(회전)과 Merge(병합)의 개념이 사용된다.
    ### Rotation (회전)
    + 원소가 삭제되는 노드를 기준으로, 삭제되는 노드의 형제노드의 원소 하나를 부모 노드로, 부모 노드의 원소 하나를 자신의 노드로 가져와 회전시키는 것을 의미한다.
    + 삭제되는 노드의 형제 노드중 3-Node가 있는 경우 회전(Rotation)이 가능하다.

    ### Merge (병합)
    + 원소가 삭제되는 노드의 기준으로, 삭제되는 노드의 직후 형체노드(혹은 직전 형제노드)와 자신 사이에 포함되는 부모 원소를 합치는 것을 의미한다.
    + merge 이후 노드의 용량이 초과되었다면 Split을 수행한다
    + 삭제되는 노드의 형제 노드중 3-Node가 없는 경우 병합(Merge)이 필요하다.

