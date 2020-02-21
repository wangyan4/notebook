### 指令

	1.  v-cloak 解决插值表达式闪烁
   	2.  v-text 默认没有闪烁
            	3.  v-bind 用来绑定属性   `v-bind:title=""` 简写 `：title=""`
         	4.  v-html 用来展示html结构字符串
            	5.  v-on 用来绑定事件 click事件 鼠标事件等
                                        	1.  事件修饰符 **可以串联且先后顺序无关(.stop.prevent)**
         
             + .stop阻止事件冒泡
             + .prevent阻止默认行为
             + .capture实现捕获触发事件的机制
             + .self 只有自身出发的时候执行回调(冒泡、捕获不会触发回调) 
             + self只会组织自己的冒泡行为不会真正阻止冒泡行为
             + .once 只触发一次
 	6.  v-model 双向数据绑定 **唯一** 

	7.
