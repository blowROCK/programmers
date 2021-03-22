## 완주하지 못한 선수

### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항
* 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
* completion의 길이는 participant의 길이보다 1 작습니다.
* 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
* 참가자 중에는 동명이인이 있을 수 있습니다.

### 입출력 예
|n|participant|completion|return|
|:-:|:-|:-|:-|
|1|["leo", "kiki", "eden"]|["eden", "kiki"]|"leo"|
|2|["marina", "josipa", "nikola", "vinko", "filipa"]|["josipa", "filipa", "marina", "nikola"]|"vinko"|
|3|["mislav", "stanko", "mislav", "ana"]|["stanko", "ana", "mislav"]|	"mislav"|

### 입출력 예 설명
#### 예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

#### 예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

#### 예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.


<hr/>

## 내 솔루션
* 그냥 틀린거부터 찾기 위해 `sort()` 돌려서 비교한다.
```javascript
function solution(participant, completion) {
    participant.sort();
    completion.sort();
    for (let i = 0; i < participant.length; i++) {
      if (participant[i] !== completion[i]) {
        return participant[i];
      }
    }
}

//효율성 fail
function solution2(participant, completion) {
  for(let i = 0 ; i < completion.length ; i++){
    const t  = participant.indexOf(completion[i]);
    if(t >= 0){
      participant.splice(t, 1)
    }
  }
  return participant.join();
}
```
<hr/>

## 감상평
* 원래는 `solution2`에서 `indexOf로` 찾아서 잘라 내는 방식 (끝까지 돈다)로 풀었는데 느리다고 빠꾸먹었다.
* `solution`는 틀린거 부터 찾으려면 어떻게 해야할까 하다가 생각난 방식.
* 쉬운 만큼 빨리 풀어야하는데, 효율성 테스트가 있는줄은 몰랐네...
