문제 : https://www.acmicpc.net/problem/2579

제일 눈에 띄는 규칙은 연속된 세 개의 계단을 모두 밟아서는 안 되는 것이다. 이 규칙으로 인해서 일차원 배열의 DP로는 해결할 수 없게된다. 

내가 생각한 방법은 2차원 배열을 선언하여 이어서 더 밟을 수 있는 계단의 수를 추가하는 것이다. 

||0|1|
|---|---|---|
|0|0|0|
|10|0|10|
|20|-|-|
|15|-|-|
|25|-|-|
|10|-|-|
|20|-|-|  

위 테이블은 문제에 예제를 보고 작성하였다. 

시작점과 첫번째 계단의 최댓값은 직접 넣어줘야 한다.   
시작점은 모두 0으로 초기화해주고 첫번째 계단의 1줄의 값을 첫번째 계단의 점수로 초기화한다. 첫번째 계단의 0줄의 값은 존재할 수 없기 때문에 10보다 작은 값인 0으로 초기화 한다.

0 줄에 해당하는 경우는 바로 전 계단을 밟아 다음 계단을 밟을 수 없는 상태이고,  
1 줄에 해당하는 경우는 처음 계단을 밟았거나 전 단계에서 두 계단을 한 번에 넘었기 때문에 다음 계단을 이어서 밟을 수 있는 상태이다.

0 줄에 올 수 있는 경우의 수는 전 계단을 밟은 상태에서 다음 계단(현재 계단)을 밟은 상태이다. 따라서 전 계단의 1줄 값에서 현재 계단의 점수를 더한 값이 현재 계단의 0줄의 값이다.

```
DP[i][0] = DP[i-1][1] + score[i]
```

1 줄에 올 수 있는 경우의 수는 두 계단을 한 번에 건너오는 경우의 수 밖에 없다. 따라서 전전 계단에서의 0, 1 중에 큰 값이랑 넘어올 계단의 점수를 더 한 값이 현재 계단의 1줄의 값이다.

```
DP[i][1] = max(DP[i-1][0], DP[i-1][1]) + score[i]
```

이 식을 근간으로 작성한 코드이다.

```js
const solution = (list) =>{
    const DP = Array.from(Array(list.length+1), () => Array(2).fill(0))
    
    DP[1][1] = list[0];
    for(let i=2;i<=list.length;i++){
        DP[i][0] = DP[i-1][1] + list[i-1];
        if(i-2 >= 0)
            DP[i][1] = Math.max(DP[i-2][0], DP[i-2][1]) + list[i-1];
    }
    
    console.log(Math.max(...DP[list.length]));
}

const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

let input = [];

rl.on('line', function (line) {
  input.push(line)
})
  .on('close', function () {
      const n = parseInt(input[0]);
      const list = [];
      
      for(let i=1;i<=n;i++){
          list.push(parseInt(input[i]));
      }
      solution(list);
      
  process.exit();
}); 
```

후기 : 이 문제를 풀기 전에 DP 문제를 몇개를 더 풀어봤었다. 풀이 방식은 금방 떠올릴 수 있었지만 이상하게 코드로 구현하는 것이 조금 헷갈렸다.