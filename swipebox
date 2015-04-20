(function($){
/****************************************************************
 * 移动端DIV左右切换插件  需结合jquery.transit.min.js使用
 * @version 1.0
 * @author liyu
*****************************************************************/
$.fn.swipebox=function(options){
	var defaults={
		index:0,//开始索引值
		loop:1,//循环播放
		callback:null//回调函数
	};
	var options=$.extend({}, defaults, options);
	return this.each(function(){
		var opt=options,
			obj=$(this),
			$childdiv=obj.find('.boxcell'),
			lendiv=$childdiv.length,
			currindex=opt.index,
			preindex=0,
			nextindex=0,
			$target=document.getElementById(obj.attr('id')),
			targetW=obj.width();
			
		var animateFlag=true,startFlag=false,moveFlag=false,touchStartX=0,touchStartY=0,touchDeltaX=0,touchDeltaY=0,touchMaxX=50;
		
		init();//初始化配置和事件绑定
		function init(){
			$childdiv.css({
				'position':'absolute',
				'width':'100%',
				'height':'100%',
				'left':0,
				'top':0,
				'display':'none'
			});
			$childdiv.eq(currindex).show();
			if(typeof opt.callback=='function') opt.callback(currindex);
			$target.addEventListener('touchstart', touchStart, false);
			$target.addEventListener('touchmove', touchMove, false);
			$target.addEventListener('touchend', touchEnd, false);
		}
		//拖拽移动
		function dragMove(x, y){
			touchDeltaX=touchStartX-x;//X的位移
			touchDeltaY=touchStartY-y;//Y的位移			
			
			//横向切换			
			if(opt.loop==1){
				if(Math.abs(touchDeltaX)>Math.abs(touchDeltaY)){
					if(touchDeltaX>0){
						//手指向左移动
						preindex=currindex-1;
						nextindex=(currindex<lendiv-1) ? currindex+1 : 0;
						$childdiv.eq(currindex).css({x:-touchDeltaX});
						$childdiv.eq(preindex).hide();
						$childdiv.eq(nextindex).show().css({x:targetW-touchDeltaX});
						moveFlag=true;
					}else{
						//手指向右移动
						preindex=currindex+1;				
						nextindex=(currindex>0) ? currindex-1 : lendiv-1;
						$childdiv.eq(currindex).css({x:-touchDeltaX});	
						$childdiv.eq(preindex).hide();
						$childdiv.eq(nextindex).show().css({x:-targetW-touchDeltaX});					
						moveFlag=true;
					}
				}
			}else{
				if(Math.abs(touchDeltaX)>Math.abs(touchDeltaY)){
					if(touchDeltaX>0){
						//手指向左移动
						if(currindex>=lendiv-1){
							$page.eq(currindex).css({x:0});
							$page.eq(currindex-1).hide();
							moveFlag=false;
						}else{
							preindex=currindex-1;
							nextindex=(currindex<lendiv-1) ? currindex+1 : 0;
							$childdiv.eq(currindex).css({x:-touchDeltaX});
							$childdiv.eq(preindex).hide();
							$childdiv.eq(nextindex).show().css({x:targetW-touchDeltaX});
							moveFlag=true;
						}						
					}else{
						//手指向右移动
						if(currindex<=0){
							$page.eq(currindex).css({x:0});
							$page.eq(currindex+1).hide();
							moveFlag=false;
						}else{
							preindex=currindex+1;				
							nextindex=(currindex>0) ? currindex-1 : lendiv-1;
							$childdiv.eq(currindex).css({x:-touchDeltaX});	
							$childdiv.eq(preindex).hide();
							$childdiv.eq(nextindex).show().css({x:-targetW-touchDeltaX});					
							moveFlag=true;
						}						
					}
				}
			}
			
		}
		//成功移动
		function moveSuccess(){
			if(touchDeltaX>0){
				//手指向左移动
				$childdiv.eq(currindex).transition({x:-targetW},function(){
					$(this).hide();
				});
				nextindex=(currindex<lendiv-1) ? currindex+1 : 0;
				$childdiv.eq(nextindex).transition({x:0},function(){
					currindex=nextindex;
					animateFlag=true;
					if(typeof opt.callback=='function') opt.callback(currindex);
				});
			}else{				
				//手指向右移动
				$childdiv.eq(currindex).transition({x:targetW},function(){
					$(this).hide();
				});				
				nextindex=(currindex>0) ? currindex-1 : lendiv-1;
				$childdiv.eq(nextindex).transition({x:0},function(){
					currindex=nextindex;
					animateFlag=true;
					if(typeof opt.callback=='function') opt.callback(currindex);
				});
			}
		}
		//移动失败
		function moveFail(){
			if(touchDeltaX>0){
				//手指向左移动
				$childdiv.eq(currindex).transition({x:0},function(){
					animateFlag=true;
				});
				nextindex=(currindex<lendiv-1) ? currindex+1 : 0;
				$childdiv.eq(nextindex).transition({x:targetW},function(){
					$(this).hide();					
				});
				$childdiv.eq(currindex-1).hide();
			}else{				
				//手指向右移动
				$childdiv.eq(currindex).transition({x:0},function(){
					animateFlag=true;
				});		
				nextindex=(currindex>0) ? currindex-1 : lendiv-1;
				$childdiv.eq(nextindex).transition({x:-targetW},function(){
					$(this).hide();	
				});
				$childdiv.eq(currindex+1).hide();
			}
		}
		//touchStart
		function touchStart(event){
			if(animateFlag){
				touchStartX=event.touches[0].pageX;//记录初始触点X
				touchStartY=event.touches[0].pageY;//记录初始触点Y
				startFlag=true;
			}
		}
		//touchMove
		function touchMove(event){
			event.preventDefault();
			if(animateFlag && startFlag){
				var touchMoveX=event.touches[0].pageX;
				var touchMoveY=event.touches[0].pageY;
				dragMove(touchMoveX, touchMoveY);
			}
		}
		//touchEnd
		function touchEnd(event){
			if(animateFlag && startFlag && moveFlag){
				animateFlag=false;
				startFlag=false;
				moveFlag=false;
				if(Math.abs(touchDeltaX)>=touchMaxX){
					moveSuccess();
				}else{
					moveFail();
				}
			}
		}
	});
};
})(jQuery);
