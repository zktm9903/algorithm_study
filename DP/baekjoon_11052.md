문제 : https://www.acmicpc.net/problem/11052

우선 1차원 DP 배열을 생성한다. 
```
DP[카드 갯수] = 최댓값
```

점화식을 도출해보자
```js
DP[1] = P[1]
DP[2] = max(DP[1] + DP[1], P[2]);
DP[3] = max(DP[1] + DP[2], P[3]);
DP[4] = max(DP[1] + DP[3], DP[2] + DP[2], P[4]);
```

위 식을 보면 현재의 카드 갯수에 대한 최댓값을 구하기 위해서 필요한 값들이 이전의 계산에서 이미 도출되었다.   

위 점화식을 토대로 작성한 코드이다.

```js
const solution = (n, list) =>{
    const DP = new Array(n+1).fill(0);
    DP[1] = list[0];
    
    for(let i=2;i<=n;i++){
        DP[i] = list[i-1];
        
        for(let j=0;j<=i;j++){
            DP[i] = Math.max(DP[i], DP[j] + DP[i-j]);
        }
    }
    
    console.log(Math.max(...DP));
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
      
      const list = input[1].split(' ').map(el=>parseInt(el));
      
      solution(n, list);
  process.exit();
}); 
```

후기 : DP에 대한 감을 어느 정도 잡은 것 같다. 굳