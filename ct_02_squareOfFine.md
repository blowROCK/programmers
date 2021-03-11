## 멀쩡한 사각
### 문제 설명
가로 길이가 Wcm, 세로 길이가 Hcm인 직사각형 종이가 있습니다. 종이에는 가로, 세로 방향과 평행하게 격자 형태로 선이 그어져 있으며, 모든 격자칸은 1cm x 1cm 크기입니다. 이 종이를 격자 선을 따라 1cm × 1cm의 정사각형으로 잘라 사용할 예정이었는데, 누군가가 이 종이를 대각선 꼭지점 2개를 잇는 방향으로 잘라 놓았습니다. 그러므로 현재 직사각형 종이는 크기가 같은 직각삼각형 2개로 나누어진 상태입니다. 새로운 종이를 구할 수 없는 상태이기 때문에, 이 종이에서 원래 종이의 가로, 세로 방향과 평행하게 1cm × 1cm로 잘라 사용할 수 있는 만큼만 사용하기로 하였습니다.
가로의 길이 W와 세로의 길이 H가 주어질 때, 사용할 수 있는 정사각형의 개수를 구하는 solution 함수를 완성해 주세요.

### 제한 사항
W, H : 1억 이하의 자연수


### 입출력 예
| W | H | result |
|:-:|:-:|:-:|
|8|12|80

### 입출력 예 설명
가로가 8, 세로가 12인 직사각형을 대각선 방향으로 자르면 총 16개 정사각형을 사용할 수 없게 됩니다. 원래 직사각형에서는 96개의 정사각형을 만들 수 있었으므로, 96 - 16 = 80 을 반환합니다.

![](https://grepp-programmers.s3.amazonaws.com/files/production/ee895b2cd9/567420db-20f4-4064-afc3-af54c4a46016.png)


<hr/>

## 솔루션

```javascript
function solution(width, hight) {
  let w = width;
  let h = hight;
  let GCDs = [];
  
  let divider = 2;
  while(divider <= w && divider <= h){
    if(w % divider === 0 && h % divider === 0){
      w = w/divider;
      h = h/divider;
      GCDs.push(divider);
      divider = 1;
    }
    divider++;
  }
  
  return (width * hight) - (width + hight - GCDs.reduce((a,b) => a * b, 1));
}
const t1 = solution(8, 12);
console.log(t1);

```
<hr/>

## 감상평
![](https://images.velog.io/images/scriptkid/post/8af35de1-119a-4d5e-a0c9-eeea4c663cb4/KakaoTalk_Photo_2021-03-11-13-30-12.jpeg)
`총사각형의수 - (w + h - (w와 h의 최대공약수))`가 정답이라는 식을 알아내는데 시간이 많이 들었다.
만약에 실제 코딩테스트에 만났다면 이 문제는 포기했을 가능성이 높다.
어릴 때, 소인수분해 하던 것이 떠올라 이 방식으로 문제를 풀었고 최대공약수를 구하는 간단한 유클리드 호제법이라는게 있는 것은 알았지만 공식이 떠오르지 않아 포기ㅠ

```javascript
//최대공약수를 구하는 간단한 재귀함수
function gcd (a, b) {
	return !b ? a : gcd( b, a % b )
}
```
