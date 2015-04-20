(function($){
/****************************************************************
 * 移动端页面上下切换插件  需结合jquery.transit.min.js使用
 * @version 1.0
 * @author liyu
*****************************************************************/
$.fn.swipepage=function(options){
	var defaults={
		index:0,//开始索引值
		loop:1,//是否循环
		switchs:'default',// 切换方式default/cover/scale
		callback:null//回调函数
	};
	var options=$.extend({}, defaults, options);
	return this.each(function(){
		var opt=options,
			obj=$(this),
			$page=obj.find('.m-page'),
			pagelen=$page.length,
			currindex=opt.index,
			preindex=0,
			nextindex=0,
			$target=document.getElementById(obj.attr('id')),
			targetH=obj.height(),
			scaling=0;
			
		var animateFlag=true,startFlag=false,moveFlag=false,touchStartX=0,touchStartY=0,touchDeltaX=0,touchDeltaY=0,touchMaxY=50;
		
		init();//初始化配置和事件绑定
		function init(){
			$page.eq(currindex).addClass('z-current');
			$target.addEventListener('touchstart', touchStart, false);
			$target.addEventListener('touchmove', touchMove, false);
			$target.addEventListener('touchend', touchEnd, false);
			if(typeof opt.callback=='function') opt.callback(currindex,currindex-1);
		}
		//拖拽移动
		function dragMove(x, y){
			touchDeltaX=touchStartX-x;//X的位移
			touchDeltaY=touchStartY-y;//Y的位移
			
			//竖向切换
			if(opt.loop==1){
				//循环切换
				if(Math.abs(touchDeltaY)>Math.abs(touchDeltaX)){					
					if(touchDeltaY>0){
						//手指向上移动					
						preindex=currindex-1;
						nextindex=(currindex<pagelen-1) ? currindex+1 : 0;
						if(opt.switchs=='cover'){
							$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY});	
						}else if(opt.switchs=='scale'){
							scaling=(1-Math.abs(touchDeltaY/targetH)).toFixed(6);
							$page.eq(currindex).css({y:-touchDeltaY,scale:scaling});
							$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY-targetH*(1-scaling)/2});	
						}else{
							$page.eq(currindex).css({y:-touchDeltaY});
							$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY});	
						}
						$page.eq(preindex).removeClass('z-move');		
						moveFlag=true;
					}else{
						//手指向下移动	
						preindex=(currindex==pagelen-1) ? 0 : currindex+1;								
						nextindex=(currindex>0) ? currindex-1 : pagelen-1;
						if(opt.switchs=='cover'){							
							$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY});
						}else if(opt.switchs=='scale'){
							scaling=(1-Math.abs(touchDeltaY/targetH)).toFixed(6);
							$page.eq(currindex).css({y:-touchDeltaY,scale:scaling});
							$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY+targetH*(1-scaling)/2});	
						}else{
							$page.eq(currindex).css({y:-touchDeltaY});							
							$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY});
						}		
						$page.eq(preindex).removeClass('z-move');
						moveFlag=true;
					}
				}
			}else{
				//不循环切换
				if(Math.abs(touchDeltaY)>Math.abs(touchDeltaX)){
					if(touchDeltaY>0){
						//手指向上移动
						if(currindex>=pagelen-1){
							$page.eq(currindex).css({y:0});
							$page.eq(currindex-1).removeClass('z-move');
							moveFlag=false;
						}else{
							preindex=currindex-1;
							nextindex=(currindex<pagelen-1) ? currindex+1 : 0;
							if(opt.switchs=='cover'){
								$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY});	
							}else if(opt.switchs=='scale'){
								scaling=(1-Math.abs(touchDeltaY/targetH)).toFixed(6);
								$page.eq(currindex).css({y:-touchDeltaY,scale:scaling});
								$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY-targetH*(1-scaling)/2});	
							}else{
								$page.eq(currindex).css({y:-touchDeltaY});
								$page.eq(nextindex).addClass('z-move').css({y:targetH-touchDeltaY});	
							}
							$page.eq(preindex).removeClass('z-move');				
							moveFlag=true;
						}						
					}else{
						//手指向下移动
						if(currindex<=0){
							$page.eq(currindex).css({y:0});
							$page.eq(currindex+1).removeClass('z-move');
							moveFlag=false;
						}else{
							preindex=currindex+1;								
							nextindex=(currindex>0) ? currindex-1 : pagelen-1;
							if(opt.switchs=='cover'){							
								$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY});
							}else if(opt.switchs=='scale'){
								scaling=(1-Math.abs(touchDeltaY/targetH)).toFixed(6);
								$page.eq(currindex).css({y:-touchDeltaY,scale:scaling});
								$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY+targetH*(1-scaling)/2});	
							}else{
								$page.eq(currindex).css({y:-touchDeltaY});							
								$page.eq(nextindex).addClass('z-move').css({y:-targetH-touchDeltaY});
							}
							$page.eq(preindex).removeClass('z-move');
							moveFlag=true;
						}
					}
				}				
			}
		}
		//成功移动
		function moveSuccess(){
			if(touchDeltaY>0){
				//手指向上移动
				preindex=currindex;
				nextindex=(currindex<pagelen-1) ? currindex+1 : 0;
				if(opt.switchs=='cover'){
				}else if(opt.switchs=='scale'){
					$page.eq(currindex).transition({y:-targetH+targetH*(1-scaling)/2},function(){
						$(this).removeClass('z-current').css({scale:1});
					});
				}else{
					$page.eq(currindex).transition({y:-targetH},function(){
						$(this).removeClass('z-current');
					});
				}				
				$page.eq(nextindex).transition({y:0},function(){
					if(opt.switchs=='cover'){
						$page.eq(currindex).removeClass('z-current');
					}
					$(this).removeClass('z-move').addClass('z-current');
					currindex=nextindex;
					animateFlag=true;
					if(typeof opt.callback=='function') opt.callback(currindex,preindex);
				});
			}else{				
				//手指向下移动
				preindex=currindex;
				nextindex=(currindex>0) ? currindex-1 : pagelen-1;
				if(opt.switchs=='cover'){
				}else if(opt.switchs=='scale'){
					$page.eq(currindex).transition({y:targetH-targetH*(1-scaling)/2},function(){
						$(this).removeClass('z-current').css({scale:1});
					});
				}else{
					$page.eq(currindex).transition({y:targetH},function(){
						$(this).removeClass('z-current');
					});	
				}							
				$page.eq(nextindex).transition({y:0},function(){
					if(opt.switchs=='cover'){
						$page.eq(currindex).removeClass('z-current');
					}
					$(this).removeClass('z-move').addClass('z-current');
					currindex=nextindex;
					animateFlag=true;
					if(typeof opt.callback=='function') opt.callback(currindex,preindex);
				});
			}
		}
		//移动失败
		function moveFail(){
			if(touchDeltaY>0){
				//手指向上移动
				preindex=currindex-1;
				nextindex=(currindex<pagelen-1) ? currindex+1 : 0;
				if(opt.switchs=='scale'){
					$page.eq(currindex).transition({y:0,scale:1},function(){
						animateFlag=true;
					});
				}else{
					$page.eq(currindex).transition({y:0},function(){
						animateFlag=true;
					});
				}				
				$page.eq(preindex).removeClass('z-move');
				$page.eq(nextindex).transition({y:targetH},function(){
					$(this).removeClass('z-move');					
				});
			}else{				
				//手指向下移动
				preindex=(currindex==pagelen-1) ? 0 : currindex+1;								
				nextindex=(currindex>0) ? currindex-1 : pagelen-1;
				if(opt.switchs=='scale'){
					$page.eq(currindex).transition({y:0,scale:1},function(){
						animateFlag=true;
					});
				}else{
					$page.eq(currindex).transition({y:0},function(){
						animateFlag=true;
					});
				}					
				$page.eq(preindex).removeClass('z-move');
				$page.eq(nextindex).transition({y:-targetH},function(){
					$(this).removeClass('z-move');					
				});
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
				if(Math.abs(touchDeltaY)>=touchMaxY){
					moveSuccess();
				}else{
					moveFail();
				}
			}
		}
	});
};
})(jQuery);
