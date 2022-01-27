## 抓球小游戏

今天在油管看到李永乐老师的视频 [街头抓珠子游戏](https://www.youtube.com/watch?v=nXDpimkn7vM)

### 大概规则
- 把红、绿、蓝各8个珠子放入一个大箱子
- 玩家一次抓取12个
- 12个球对应颜色的数字，按照奖罚表兑奖

### 奖罚表
| 数字 | 奖罚  |
| ---- | ----- |
| 840  | 100元 |
| 831  | 10元  |
| 822  | 10元  |
| 750  | 20元  |
| 741  | 2元   |
| 732  | 2元   |
| 660  | 20元  |
| 651  | 1元   |
| 642  | 1元   |
| 633  | 1元   |
| 552  | 1元   |
| 444  | 1元   |
| 543  | -10元  |

根据视频所说，最后这个543是个坑，概率很高。看着很简单，于是想起用程序实现一下。

### 逻辑脚本 ballgame.js

##### 初始化
```js
function init() {
	 ballArr = [] // 球箱
	 newArr = [] // 抓球结果
	 redArr = [] // 红球结果
	 greenArr = [] // 绿球结果
	 blueArr = [] // 蓝球结果
	 for (let i = 0; i < 8; i++) {
		 ballArr.push('r'+i)
		 ballArr.push('g'+i)
		 ballArr.push('b'+i)
	 }
}
```

##### 随机抓12个球
```js
function getBall(arr) {
 	if (arr.length < 12) {
		const rndnum = getRandomInt(ballArr.length)
		const rndball = ballArr[rndnum]
		if (!newArr.includes(rndball)) {
			newArr.push(rndball)
			if (rndball.startsWith('r')) redArr.push(rndball)
			if (rndball.startsWith('g')) greenArr.push(rndball)
			if (rndball.startsWith('b')) blueArr.push(rndball)
		 }
		getBall(newArr)
 	} else {
 		// console.log('Finished');
 	}
}
```

##### 返回抓球结果
```js
const getBallResult = () => {
 	init()
 	getBall(newArr)
 	const rtArr = [redArr.length, greenArr.length, blueArr.length]
 	return rtArr.sort().reverse().join('')
}
```

##### 返回奖罚金额
```js
const matchTb = {
 '840': 100,
 '831': 10,
 '822': 10,
 '750': 20,
 '741': 2,
 '732': 2,
 '660': 20,
 '651': 1,
 '642': 1,
 '633': 1,
 '552': 1,
 '444': 1,
 '543': -10
}
const resultMoney = (rt) => {
 	return matchTb[rt]
}
```

### 调用脚本 ballgametest.js
```js
const {getBallResult, resultMoney} = require('./lib/ballgame.js')

const count = 100 // 抓球次数
let bossMoney = 0 // 奖罚金额

const playGame = (ct)=>{
	 if(ct > 0){
		 ct--
		 let promise = new Promise(function(resolve, reject) {
			 const result = getBallResult()
			 resolve(result);
		 });
		 promise.then(result=>{
			 const money = resultMoney(result)
			 bossMoney += money
			 console.log(count-ct, result, money, bossMoney);
			 playGame(ct)
		 })
	}else{
		 console.log('Finished');
	}
}
playGame(count)
```

##### 代码地址
https://github.com/weblusky/jsgame