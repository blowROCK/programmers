## 스킬트리
### 문제 설명
선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더일때`, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. `따라서 스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크나 라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.


### 제한 조건

* 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
* 스킬 순서와 스킬트리는 문자열로 표기합니다.
    * 예를 들어, C → B → D 라면 "CBD"로 표기합니다
* 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
* skill_trees는 길이 1 이상 20 이하인 배열입니다.
* skill_trees의 원소는 스킬을 나타내는 문자열입니다.
    * skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.



### 입출력 예
| skill | skill_trees | return |
|:-:|:-:|:-:|
|"CBD"|["BACDE", "CBADF", "AECB", "BDA"] | 2|


### 입출력 예 설명
* "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
* "CBADF": 가능한 스킬트리입니다.
* "AECB": 가능한 스킬트리입니다.
* "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.



<hr/>

## 내 솔루션

내가 푼 방식은 아래와 같다.
* `["BACDE", "CBADF", "AECB", "BDA"]`에서 `"CBD"`와 매칭되는 글자를 해당 인덱스로 바꾼다.
    * `["102", "012", "01", "12"]`
* 이 숫자가 0부터 오름차순의 순서이면 true이다.


```javascript
function solution(skill, skill_trees) {
  const skill_arr = skill.split('');
  return skill_trees.map(skill_tree => {
    return skill_tree.split('') // 스킬 트리 배열을 글자 단위로 자름.
      .map(S => skill.split('').findIndex((el, i) => S === el)) // 스킬트리의 글자와 스킬을 비교하여 찾은 인덱스를 리턴
      .filter(el=>el !== -1).map((e,i) => e === i).every(x=>x); // 찾은 인덱스의 순서가 오름차순인지 검사함.
  }).filter(el => el).length; // 오름 차순으로 검사한 배열의 숫자를 셈
}
```
<hr/>

## 감상평

풀고 나니 `startsWith`이 있었던 것이 기억나더라 😪
다른 사람이 알아먹기도 힘들고 나 자신조차도 `split('')`한다고 정신 없었다. ㅋㅋ
다른 사람들 풀이를 보고 있자니 나는 뭔가 `map, filter`에 많이 꼿혀 있는 편인 것 같다.


<br/>
<br/>
<hr/>

## 최고의 솔루션
```javascript
function solution(skill, skill_trees) {
    let regex = new RegExp(`[^${skill}]`, 'g'); // regex 활용법
    return skill_trees
        .map((x) => x.replace(regex, ''))
        .filter((x) => skill.startsWith(x)) // startsWith 기억해둘것
        .length
}
// 다른 사람 풀이에 조금 수정만 했다.
// skill.indexOf(x) === 0 로도 가능.
```







