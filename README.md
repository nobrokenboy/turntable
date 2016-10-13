##分析当前目录下的三个html实现不同思路，不同UI的抽奖
###1.index.html
####描述：简单的抽奖界面，纯canvas以及js实现,适配于网页，借用他人的思路进行模仿实现
####思路：
#####1.1canvas绘制UI界面；
#####1.2根据角度旋转知识，加入缩减速率控制随机；
![canvas.gif](https://github.com/nobrokenboy/turntable/blob/master/canvas_no_img.gif)

[戳我进入展示页面](http://nobrokenboy.me/turntable/index.html)
###2.lottery_draw_prize.html
####描述：图片实现抽奖页面,结合jquery插件awardRotate.js，主要适配于ipad；
####思路：
#####2.1设定抽奖次数为1，在点击抽奖时先执行判断抽奖次数是不是等于0；
#####2.2抽奖中加入概率（通过后台获取），通过在[0,100]取随机数，然后判断随机数处哪个区间，最后再确定要旋转的角度；
#####2.3加入本地存储，为了防止因网络问题造成无法取到后台抽奖奖品的数据；
####缺陷：1.可能最终角度的确定还不够准确，原因是对这个终角度的计算细节上还不太懂；
####核心代码：
<pre><code>
      $('#btnArrow').click(function (){
            	//判断点击次数，规则只能限定抽奖一次
                if(clickTimes!=0){
                	clickTimes--;
                }else{
                	alert("亲，您的抽奖次数已用光！");
                	return false;
                }
                //在[1,100]获取随机数
            	var result=randomNum(1,100);
            	console.log(resultData);
            	//数字起点
            	var baseNum=1;
            	//随机数对应的奖项
            	var index=0;
            	//获取的数字与奖项对应起来
                for(var i=0,j=resultData.length;i<j;i++){
                	//获取每个奖项的概率（用百分比）
                	var chance=resultData[i].drawProb*100;
                	//判断随机数字是不是在对应奖项的概率范围内(谢谢参与不需要)
                	if(chance!=0){
                		if(result>=baseNum&&result<(baseNum+chance)){
                			index=i+1;
                			//下面是测试数据
                			console.log(index+"等奖，随机数是"+result+","+chance+"百分比");
                			
                			//根据奖项对应角度
                		    switch (index) {
			                    case 1:
			                        rotateFn(1, 88, resultData[0].awardName,resultData[0].awardDesc);
			                        break;
			                    case 2:
			                        rotateFn(2, 337, resultData[1].awardName,resultData[1].awardDesc);
			                        break;
			                    case 3:
			                        rotateFn(3, 287, resultData[2].awardName,resultData[2].awardDesc);
			                        break;
			                    case 4:
			                        rotateFn(4, 185, resultData[3].awardName,resultData[3].awardDesc);
			                        break;
			                    case 5:
			                        rotateFn(5, 162, resultData[4].awardName,resultData[4].awardDesc);
			                        break;
			                    case 6:
			                        rotateFn(6, 130, resultData[5].awardName,resultData[5].awardDesc);
			                        break;
			                }
                		}
                	}
                	//重新设置奖项起点
                	baseNum=baseNum+chance;
                }
                
            	                
            });
</pre></code>

<pre><code>
      var rotateFn = function (awards, angles, txt, prize){
                $('#rotate').stopRotate();
                $('#rotate').rotate({
                    angle:0,
                    animateTo:angles+1800,//转动5圈
                    duration:8000,
                    callback:function (){
                    	alert("恭喜您，您抽中"+txt+",获取的奖品是"+prize);

                    }
                })
            };
</pre></code>

<pre><code>
      //本地储存
      if(typeof(window.localStorage.localPrizeData)=="undefined"){
         getTurntableData();
      }else{
         resultData=JSON.parse(window.localStorage.getItem("localPrizeData"));
      }
</pre></code>

![canvas.gif](https://github.com/nobrokenboy/turntable/blob/master/img_turntable.gif)

[戳我进入展示页面](http://nobrokenboy.me/turntablelottery_draw_prize.html)
###3.lottery_draw.html
####描述：img+canvas实现抽奖页面，由于我对canvas还不够熟悉，部分复杂UI实现不了，还待继续完善；此外，判断抽奖指针最终的位置还需要再重新整理思路；
