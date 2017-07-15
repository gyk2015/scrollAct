<template>
	<div class="scrollDemo">
		<slot>
	    	<div class="container">欢迎使用</div>
	  	</slot>
	</div>
</template>

<script>
	export default {
  	name: 'srollDemo',
  	data () {
    	return {
      		
    	}
  	},
  	props: ['isVertical'],
  	mounted: function() {
  		this.initPosition();
  	},
  	methods: {
  		initPosition() {
  			let els = document.querySelector('.scrollDemo').querySelectorAll('.container');
  			if(this.isVertical) {
	  			for(let i = 0;i < els.length;i ++) {
	  				els[i].style.top = i*100 + '%';
	  				els[i].style.left = 0;
	  				this.addSroll(els[i],i+1,els.length);
	  			}
	  		}else {
	  			for(let i = 0;i < els.length;i ++) {
	  				els[i].style.left = i*100 + '%';
	  				els[i].style.top = 0;
	  				this.addSroll(els[i],i+1,els.length);
	  			}
	  		}
  		},
  		addSroll(e,page,pageNum) {
  			var _this = this;
		    var touch = e;
		    var SWIPE_DISTANCE = document.body.clientWidth/4;
		    var point_start,
		        point_end;
		    //计算两点之间距离
		    var getDist = function(p1 , p2){
		        if(!p1 || !p2) return 0;
		        return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
		    }
			//获取touch的点(兼容mouse事件)
		    var getTouchPos = function(e){
		        var touches = e.touches;
		        if(touches && touches[0]) {
		            return { x : touches[0].clientX ,
		                     y : touches[0].clientY };
		        }
		        //鼠标点击
		        return { x : e.clientX , y: e.clientY };
		    }
		    //获取swipe的方向
		    var getSwipeDirection = function(p2,p1){
		        var dx = p2.x - p1.x;
		        var dy = -p2.y + p1.y;    
		        var angle = Math.atan2(dy , dx) * 180 / Math.PI;
		        if(_this.isVertical == true) {
		        	if(angle < 45 && angle > -45) return false;
			        if(angle >= 45 && angle < 135) return true;
			        if(angle >= 135 || angle < -135) return false;
			        if(angle >= -135 && angle <= -45) return true;
		        }else {
					if(angle < 45 && angle > -45) return true;
			        if(angle >= 45 && angle < 135) return false;
			        if(angle >= 135 || angle < -135) return true;
			        if(angle >= -135 && angle <= -45) return false;
		        }
		        
		    }
		    var scorllAction = function(dvalue){
	            if(dvalue > 0){
	            	if(page < pageNum) {
	            		if(_this.isVertical) {
            				document.querySelector('.scrollDemo').style.transform = "translateY(-"+page*100+"%"+")"  
            			}else { 
            				document.querySelector('.scrollDemo').style.transform = "translateX(-"+page*100+"%"+")" 
            			}
            		}
	            }else{
	            	if(_this.isVertical) {
	            		document.querySelector('.scrollDemo').style.transform = "translateY(-"+(page-2)*100+"%"+")"
	            	}else {
	            		document.querySelector('.scrollDemo').style.transform = "translateX(-"+(page-2)*100+"%"+")" 
	            	}
	                
	            }
		    }

    		var startEvtHandler = function(e){
    			console.log('start')
		        point_start = getTouchPos(e);
		         // event.preventDefault();
		    }
		    var moveEvtHandler = function(e){
		    	console.log('move')
		        point_end = getTouchPos(e);
		        if(getSwipeDirection(point_start,point_end)) {
		        	event.preventDefault();  
		        }    
		    }
		    var endEvtHandler = function(e){ 
		    	console.log('end')
		        if(getDist(point_start,point_end) > SWIPE_DISTANCE && getSwipeDirection(point_start,point_end)){
		        	if(_this.isVertical) {
		        		scorllAction(point_start.y - point_end.y);
		        	}else {
		        		scorllAction(point_start.x - point_end.x);
		        	}
		           
		        }
		        point_start=0;
		        point_end=0;
		    }
		    
		    var startEvt, moveEvt, endEvt;
		    if("ontouchstart" in window) {
		        startEvt="touchstart";
		        moveEvt="touchmove";
		        endEvt="touchend";
		    }else {
		        startEvt="mousedown";
		        moveEvt="mousemove";
		        endEvt="mouseup";            
		    }
		    touch.addEventListener(startEvt, startEvtHandler);
    		touch.addEventListener(moveEvt, moveEvtHandler);
    		touch.addEventListener(endEvt, endEvtHandler);           
  		}
  	}
}
</script>

<style scoped>
	.scrollDemo {
		height: 100%;
		width: 100%;
		position: relative;
		backface-visibility: hidden;
		transform:translate3d(0,0,0);
		-webkit-transform:translate3d(0,0,0);
		-moz-transform:translate3d(0,0,0);
		-o-transform:translate3d(0,0,0);
		-ms-transform:translate3d(0,0,0);
		-webkit-transition:all 0.6s ease-in-out;
		-moz-transition:all 0.6s ease-in-out;
		-o-transition:all 0.6s ease-in-out;
		-ms-transition:all 0.6s ease-in-out;
		transition:all 0.6s ease-in-out;

	}
	.container {
		width: 100%;
		height: 100%;
		position: absolute;
	}	
</style>