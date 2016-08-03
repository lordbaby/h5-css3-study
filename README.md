# h5-css3-study
学习h5,css3用的，会有些自认为有用的tips

# canvas 
## 时间倒计时效果

根据慕课网liuyubobobo大神学习的，设置倒计时。

效果图：

![canvas1](/h5/canvas/images/count-down.png)

主要点有
1. 已经绘制点阵数组（0，1），代表0-9，有点图像像素点的意思

```javascript
[0,0,0,1,1,0,0],
[0,1,1,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[0,0,0,1,1,0,0],
[1,1,1,1,1,1,1]

//比如这就代表的1
//根据1来绘制圆点 
```

2. 采用setInterval定时更新数字。这些不算难点
3. 更新数字时小球散落效果这个是难点

```javascript
思路：
1. 在数字变动时，保存要更改的时间数字（时分秒，具体到每个数字） 

    function addBalls(x, y, num) {
        var digitNum = digit[num];

        for (var i = 0; i < digitNum.length; i++) {
            for (var j = 0; j < digitNum[i].length; j++) {
                if (digitNum[i][j] == 1) {
                    var aBall = {
                        x: x + j * 2 * (RADUIS + 1) + (RADUIS + 1),
                        y: y + i * 2 * (RADUIS + 1) + (RADUIS + 1),
                        g: 1.5 + Math.random(),
                        vx: Math.pow(-1, Math.ceil(Math.random() * 1000)) * 4,
                        vy: -5,
                        color: colors[Math.floor(Math.random() * colors.length)]
                    }
                    balls.push(aBall);
                }
            }
        }
    }
2. 在重新绘制数字时，把balls中的小球重新绘制到对应的时分秒
    for (var i = 0; i < balls.length; i++) {
        ctx.fillStyle = balls[i].color;
        ctx.beginPath();
        ctx.arc(balls[i].x, balls[i].y, RADUIS, 0, 2 * Math.PI, true);
        ctx.closePath();
        ctx.fill();
    }
3. 绘制完后小球的掉落效果。也在更新数字时调用

    function updateBalls() {

        for (var i = 0; i < balls.length; i++) {
            var ball = balls[i];
            ball.x += ball.vx;
            ball.y += ball.vy;
            ball.vy += ball.g;
            if (ball.y > WINDOW_HEIGHT - RADUIS) {
                ball.y = WINDOW_HEIGHT - RADUIS
                ball.vy = -ball.vy * .75
            }
        }
        //这个性能优化是学习亮点 ，循环遍历当前数组中的小球，没有出边界的重新保存到数组中，在把数组中长度大于当前保存索引的小球全部pop掉
        var cnt = 0;
        for (var i = 0; i < balls.length; i++) {
            if (balls[i].x + RADUIS > 0 && balls[i].x - RADUIS < WINDOW_WIDTH) {
                balls[cnt++] = balls[i];
            }
        }

        while (balls.length > cnt) {
            balls.pop();
        }

    }

```