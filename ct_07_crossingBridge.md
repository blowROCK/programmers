## 다리를 지나는 트럭
### 문제 설명
트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.


|경과 시간|다리를 지난 트럭|다리를 건너는 트럭|대기 트럭|
|:-:|:-|:-|:-|
|0|[]|[]|[7,4,5,6]|
|1~2|[]|[7]|[4,5,6]|
|3|[7]|[4]|[5,6]|
|4|[7]|[4,5]|[6]|
|5|[7,4]|[5]|[6]|
|6~7|[7,4,5]|[6]|[]|
|8|[7,4,5,6]|[]|[]|

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.


### 제한 조건
* bridge_length는 1 이상 10,000 이하입니다.
* weight는 1 이상 10,000 이하입니다.
* truck_weights의 길이는 1 이상 10,000 이하입니다.
* 모든 트럭의 무게는 1 이상 weight 이하입니다.

### 입출력 예
|bridge_length|weight|truck_weights|return|
|:-|:-|:-|:-|
|2|10|[7,4,5,6]|8|
|100|100|[10]|101|
|100|100|[10,10,10,10,10,10,10,10,10,10]|110|



<hr/>

## 내 솔루션
* 문제 설명에서 나오는 표를 보고 그대로 이용했다.
* 시간이 1초 지나갈 때마다 실제 트럭이나 0을 넣어 가짜 트럭을 다리에 넣어준다.
* 트럭이 다리의 마지막인 `bridge[bridge_length-1]`에 도착하면 빼낸다.
    * 만약 0이 아닌(실제트럭) 숫자가 들어오면 지나간 트럭 배열에 넣는다.
* 나머지 로직은 코드와 주석을 함께 보면된다.

```javascript
function solution(bridge_length, weight, truck_weights) {
  const trucks = JSON.parse(JSON.stringify(truck_weights));
  const passed = []; // 지나간 트럭 배열.
  const bridge = []; // 다리
  let bridgeWeight = 0; // 지금 다리의 무게
  let count = 0; // 지나간 초, return 할 값.
  
  // 파라미터로 들어온 truck_weights의 길이와 지나간 트럭 배열의 길이가 다를 동안.
  while(passed.length !== truck_weights.length){
    count++;
       
    // 만약 다리 배열 bridge의 마지막이 숫자라면
    if(typeof bridge[bridge_length-1] === 'number'){
      const truck = bridge.pop(); // 마지막 트럭을 다리에서 빼낸다.
      bridgeWeight -= truck; // 트럭의 무게만큼 지금 무게에 뺀다.
      // 트럭이 0보다 크면 (실제 트럭) 지나간 트럭 배열에 넣는다.
      if(truck > 0) passed.push(truck); 
    }
    
    //지금 무게와 1번 트럭이 통과 가능하면
    if(trucks[0] + bridgeWeight <= weight){
      bridgeWeight += trucks[0]; // 트럭을 다리에 넣음.
      bridge.unshift(trucks.shift()); // 다리에서 앞으로 넣어준다.
    }else{
      // 트럭이 못지나가므로 빈트럭 0 을 넣어 준다.
      bridge.unshift(0);
    }
  }
  return count;
}
```
<hr/>

## 감상평
게임을 만들 듯이 만들게 되었다. 그러다 보니 다리가 버텨주는 무게라면 0이라는 가짜 트럭을 다리 배열`bridge`에 넣어주어서 실제로 다리를 건너 듯이 로직을 짰다. 단점이라면 무게가 못버티는 중에는 반복문을 스킵할 수 없고 0이라는 가짜 트럭을 넣는 다는 점이다.
