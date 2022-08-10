문제 : https://www.acmicpc.net/problem/1149

세 규칙을 한 문장으로 정리하면 '바로 옆의 집과는 색이 달라야 한다' 이다.

색의 가짓수는 3가지로 정해져 있기 때문에 2차원 배열을 선언하여 열을 3줄로 설정하면 된다. 

현재 집에 현재 색깔을 색칠하기 위한 최소값은   
전 집의 현재 색깔을 제외한 두 가지 색깔 중에 최소값이랑 현재 집의 현재 색깔의 비용을 더한 것이다.

점화식을 세워보자.

```js
DP[i][0] = Math.min(DP[i-1][1], DP[i-1][2]) + list[i][0];
DP[i][1] = Math.min(DP[i-1][0], DP[i-1][2]) + list[i][1];
DP[i][2] = Math.min(DP[i-1][0], DP[i-1][1]) + list[i][2];
```

첫번째 집의 각 색깔별 최소값은 첫번째 집의 각 색깔별 비용으로 초기화해준다.

```js
const solution = (n, list) =>{
    const DP = Array.from(Array(n), () => Array(3).fill(0));
    
    DP[0] = list[0];
    
    for(let i=1;i<n;i++){
        DP[i][0] = Math.min(DP[i-1][1], DP[i-1][2]) + list[i][0];
        DP[i][1] = Math.min(DP[i-1][0], DP[i-1][2]) + list[i][1];
        DP[i][2] = Math.min(DP[i-1][0], DP[i-1][1]) + list[i][2];
    }

    console.log(Math.min(...DP[n-1]))
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
          list.push(input[i].split(' ').map(el=>parseInt(el)));
      }
      
      solution(n, list);
  process.exit();
}); 
```

후기 : 문제 푸는 시간이 많이 단축됐다. 생각하는 시간, 코드 짜는 시간을 합해서 5분이 안걸렸던 같다. 기부니가 좋다..