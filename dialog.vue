<template>
	 <div v-show="show" class="xa-popup">
	  	<div class="layer"></div>
	  	<div ref="popup" class="popup">
	  		<div ref="title" :class="title.className || 'xa-bg-eee'" class="title">
	  			<div class="name">{{ title.text || title }}</div>
	  			<div @click="close()" class="close"></div>
	  		</div>
	  		<div ref="main" class="main"><slot></slot></div>
			<div v-show="buttons.length" class="buttons">
				<div v-for="btn in buttons" @click="setEvent(btn)" :class="btn.className" class="button">{{ btn.text }}</div>
			</div>
		</div>
	</div>
</template>

<script>
var drag = (dragBox,moveBox = dragBox) => {
    dragBox.onmousedown = e => {
        var disX=e.clientX-moveBox.offsetLeft;
        var disY=e.clientY-moveBox.offsetTop;
        document.onmousemove = (e) =>{
            e.preventDefault();
            var l=e.clientX-disX;
            var t=e.clientY-disY;
            var x = document.documentElement.clientWidth-moveBox.offsetWidth;
            var y = document.documentElement.clientHeight-moveBox.offsetHeight;
            l = l < 0 ? 0 : (l > x ? x : l);
            t = t < 0 ? 0 : (t > y ? y : t);
            moveBox.style.left=l+'px';
            moveBox.style.top=t+'px';
            return false;
        }
        document.onmouseup = () => {
            document.onmousemove = null;    
            document.onmouseup = null;
            return false;
        }
        return false;
    }
}
export default {
	methods:{
		// 弹框适配屏幕居中
		autosize(){
			this.$nextTick( () => {
				var dom = this.$refs.popup;
				var CHeight = document.documentElement.clientHeight;
				var CWidth = document.documentElement.clientWidth;
				var PHeight = dom.offsetHeight;
				var PWidth = dom.offsetWidth;
				console.log(PHeight,PWidth)
				this.$refs.main.style.maxHeight = CHeight - 100 +'px';
				dom.style.top = (CHeight>PHeight?(CHeight-PHeight)/2:0) +'px';
				dom.style.left = (CWidth-PWidth)/2 +'px';
			})
        },
        setEvent (btn) {
        	this.$emit(btn.clickEvent)
        	this.$emit('onbtnclick', btn)
        },
		close () {
			this.$emit('input', false)
		}
    },
	props: {
		title: {
			default: ''
		},
		value: {
			type: Boolean,
			default: false
		},
		buttons: {
			type: Array,
			default () {
				return []
			}
		}
	},
	computed: {
		show () {
			return this.value
		}
	},
	watch: {
		show (val) {
			val && this.autosize();
		}
	},
	mounted () {
		this.$nextTick( () => {
			drag(this.$refs.title, this.$refs.popup);
		})
	}
}
</script>

<style scoped>
	/*dialog*/
	.xa-popup .layer{position:fixed;left:0;top:0;background:rgba(0,0,0,0.3);width:100%;height:100%;z-index:100;}
	.xa-popup .popup{min-width:300px;background:#fff;z-index:101;position:fixed;left:50%;top:50%;box-shadow:0 0 10px #888;border-radius:5px;overflow:hidden;}
	.xa-popup .popup>.title{width:100%;height:44px;line-height:44px;text-align:center;font-size:16px;cursor:move;position:relative;}
	.xa-popup .popup>.title>.close{height:44px;width:44px;position:absolute;right:0;top:0;cursor:pointer;font-size:30px;}
	.xa-popup .popup>.title>.close:after{content:'×';display:block;}
	.xa-popup .popup>.title>.close:hover{background:rgba(255,255,255,0.1);}
	.xa-popup .popup>.main{overflow-y:auto;overflow-x:hidden;}
	.xa-popup .popup>.buttons{display:flex;justify-content:center;padding:10px 10px;box-shadow:0 0 6px #ccc;}
	.xa-popup .popup>.buttons .button{flex:1;max-width:160px;text-align:center;cursor:pointer;height:36px;line-height:36px;box-shadow:0 0 1px #ccc;margin:0 10px;border-radius:5px;}
	/* theme库 */
	.xa-bg-eee{background:#eee;color:#333;}
	.xa-bg-red,.xa-bg-red-click{background-color:#F33A3A;color:white;}
	.xa-bg-orange,.xa-bg-orange-click{background-color:#FA9E3B;color:white;}
	.xa-bg-yellow,.xa-bg-yellow-click{background-color:#fff600;color:white;}
	.xa-bg-green,.xa-bg-green-click{background-color:#009000;color:white;}
	.xa-bg-blue,.xa-bg-blue-click{background-color:#268dff;color:white;}
	.xa-bg-gray,.xa-bg-gray-click{background-color:#BCBCBC;}
	.xa-bg-white,.xa-bg-white-click{background-color:white;color:#333;}
	.xa-bg-black,.xa-bg-black-click{background-color:black;color:white;}

	.xa-bg-red-click:hover{background-color:#EF4F4F;}
	.xa-bg-orange-click:hover{background-color:#FAAE4B;}
	.xa-bg-yellow-click:hover{background-color:#ffff00;}
	.xa-bg-green-click:hover{background-color:#009900;}
	.xa-bg-blue-click:hover{background-color:#007ae2;}
	.xa-bg-gray-click:hover{background-color:#BCB6BC;}
	.xa-bg-white-click:hover{background-color:#f2f2f2;}
	.xa-bg-black-click:hover{background-color:#333;}
	.xa-click:hover{background:#f2f2f2;}
</style>