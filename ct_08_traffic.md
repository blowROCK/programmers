## 추석 트래픽
### 문제 설명
이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다. 장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인 9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다. 초당 최대 처리량은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.

### 입력 형식
* `solution` 함수에 전달되는 `lines` 배열은 N(1 ≦ N ≦ 2,000)개의 로그 문자열로 되어 있으며, 각 로그 문자열마다 요청에 대한 응답완료시간 S와 처리시간 T가 공백으로 구분되어 있다.
* 응답완료시간 S는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 `2016-09-15 hh:mm:ss.sss` 형식으로 되어 있다.
* 처리시간 T는 `0.1s`, `0.312s`, `2s` 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 `s`로 끝난다.
  예를 들어, 로그 문자열 `2016-09-15 03:10:33.020 0.011s`은 "2016년 9월 15일 오전 3시 10분 33.010초"부터 "2016년 9월 15일 오전 3시 10분 33.020초"까지 "0.011초" 동안 처리된 요청을 의미한다.** (처리시간은 시작시간과 끝시간을 포함)**
* 서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 0.001 ≦ T ≦ 3.000이다.
* `lines` 배열은 응답완료시간 S를 기준으로 오름차순 정렬되어 있다.

### 출력 형식
`solution` 함수에서는 로그 데이터 `lines` 배열에 대해 초당 최대 처리량을 리턴한다.

### 입출력 예제
#### 예제1
* 입력: [
  "2016-09-15 01:00:04.001 2.0s",
  "2016-09-15 01:00:07.000 2s"
  ]

* 출력: 1

#### 예제2
*  입력: [
   "2016-09-15 01:00:04.002 2.0s",
   "2016-09-15 01:00:07.000 2s"
   ]

* 출력: 2

* 설명: 처리시간은 시작시간과 끝시간을 포함하므로
  첫 번째 로그는 01:00:02.003 ~ 01:00:04.002에서 2초 동안 처리되었으며,
  두 번째 로그는 01:00:05.001 ~ 01:00:07.000에서 2초 동안 처리된다.
  따라서, 첫 번째 로그가 끝나는 시점과 두 번째 로그가 시작하는 시점의 구간인 01:00:04.002 ~ 01:00:05.001 1초 동안 최대 2개가 된다.

#### 예제3
* 입력: [
  "2016-09-15 20:59:57.421 0.351s",
  "2016-09-15 20:59:58.233 1.181s",
  "2016-09-15 20:59:58.299 0.8s",
  "2016-09-15 20:59:58.688 1.041s",
  "2016-09-15 20:59:59.591 1.412s",
  "2016-09-15 21:00:00.464 1.466s",
  "2016-09-15 21:00:00.741 1.581s",
  "2016-09-15 21:00:00.748 2.31s",
  "2016-09-15 21:00:00.966 0.381s",
  "2016-09-15 21:00:02.066 2.62s"
  ]

* 출력: 7

* 설명: 아래 타임라인 그림에서 빨간색으로 표시된 1초 각 구간의 처리량을 구해보면 (1)은 4개, (2)는 7개, (3)는 2개임을 알 수 있다. 따라서 초당 최대 처리량은 7이 되며, 동일한 최대 처리량을 갖는 1초 구간은 여러 개 존재할 수 있으므로 이 문제에서는 구간이 아닌 개수만 출력한다.

![](http://t1.kakaocdn.net/welcome2018/chuseok-01-v5.png)


<hr/>

## 내 솔루션
중요 포인트는
* 끝난 시간과 경과된 시간으로 시작 시간을 구해야한다.
* 처리 시간은 시작 시간과 끝시간을 포함한다.
* 저 그림이 매우 중요한 힌트다.

```javascript
// 시간을 나누고 시작과 끝. 그걸 sort한 flag 배열을 돌려받는다.
function getTimes(lines){ 
  const flags = [];
  const lineObj = lines.map(line => {
    const splitLine = line.split(' ');
    const times = splitLine[1].split(':');
    // times[2]*1을 안하면 times[2]는 string 값을 가지고 있다.
    // js개발자라면 type관련 개똥같은 상황을 자주 만나는데, 나는 방금 만났다.
    // 끝 시간은 +1을 해서 끝 시간을 포함하도록 한다.
    const end = (times[0] * 3600 + times[1] * 60 + times[2]*1) * 1000 + 1;
    const elapsed = splitLine[2].replace('s','')*1000; 
    flags.push(...[end - elapsed, end]);
    // 시작 시간은 +1을 해서 시작 시간을 포함하도록 한다.
    return { start: end - elapsed + 1, end }
  });
  return { lineObj, flags: flags.sort((a,b)=> a - b) };
}

function solution(lines) {
  const { lineObj, flags } = getTimes(lines);
  const answer = [];
  let count = 0;
  flags.map(flag => {
    count = 0;
    for(let idx = 0 ; idx < lineObj.length ; idx++){
      //제일 난관인 999를 더하는 부분. 시작과 끝에 +1씩 했으니 1001만큼 검사함. 시작인 0과 끝인 1001포함.
      if( flag + 999 >= lineObj[idx].start && flag <= lineObj[idx].end ){
        count++;
      }
    }
    answer.push(count);
  })
  return Math.max(...answer);
}
```
<hr/>

## 감상평
토요일에 갑자기 삘받아서 하나 풀자 생각하고는 가벼운 마음으로 룰루랄라 들어간 프로그래머스에서 아주 폭력적인 계산 문제를 만났다.

시간 부분을 나누고 `ms`단위로 자르는 아이디어 까지는 술술 나왔는데,
시간시작, 끝 부분을 모두 서치해야 한다는 생각은 뒤늦게 나와 코드가 `getTimes()`에서 조금 뭉쳤다.
제출하고 중간에 5번 테스트에서 오류가 나왔는데 그 이유는 `split()` 후에는 `string`이 리턴되는 것을 잊어먹고 `+times[2]`를 했기 때문..ㅠㅠ
> times[2] -> string '920'
times[2]*1 -> number 920


악독한 카카오녀석들.. 아주 잔인한 놈들임에 틀림없다. 이런게 입사 테스트용이라니 아마도 회사에 들어가면 "Hey, scriptKid!"라고 영어 이름을 부르며, 친근하게 내 뚝배기를 깨버릴 것이다.
