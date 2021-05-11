# use-ie8
兼容ie8
ie hack 
“-″减号是IE6专有的hack
“\9″ IE6/IE7/IE8/IE9/IE10都生效
“\0″ IE8/IE9/IE10都生效，是IE8/9/10的hack
“\9\0″ 只对IE9/IE10生效，是IE9/10的hack

只在IE下生效
<!--[if IE]>
这段文字只在IE浏览器显示
<![endif]-->

只在IE6下生效
<!--[if IE 6]>
这段文字只在IE6浏览器显示
<![endif]-->

只在IE6以上版本生效
<!--[if gte IE 6]>
这段文字只在IE6以上(包括)版本IE浏览器显示
<![endif]-->

只在IE8上不生效
<!--[if ! IE 8]>
这段文字在非IE8浏览器显示
<![endif]-->

非IE浏览器生效
<!--[if !IE]>
这段文字只在非IE浏览器显示
<![endif]-->
## 部分解决方案
    1、echarts兼容ie8，https://echarts.apache.org/zh/builder.html
        解决：使用5.0版本一下的echarts.js，并选择兼容ie8，这里采用4.5.0版本
    2、str.tirm()方法不兼容ie10一下，
        解决：if (!String.prototype.trim) {
                String.prototype.trim = function () {
                return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
                };
             }
    3、透明度处理
        background: rgba(9,30,66,.04);
        color: hsla(0,0%,100%,1)\9;
		filter: alpha(opacity=60)\9;

        background-color: #000000 \9;
	    filter: alpha(opacity=67)\9;
    4、echarts层级问题，给外层父级元素设置position:relative;z-index:1;(将父级元素设置为定位元素，然后设置层级)
    5、forEach()  ie8  兼容
        if ( !Array.prototype.forEach ) {
        Array.prototype.forEach = function forEach( callback, thisArg ) {
            var T, k;
            if ( this == null ) {
            throw new TypeError( "this is null or not defined" );
            }
            var O = Object(this);
            var len = O.length >>> 0; 
            if ( typeof callback !== "function" ) {
            throw new TypeError( callback + " is not a function" );
            }
            if ( arguments.length > 1 ) {
            T = thisArg;
            }
            k = 0;
            while( k < len ) {
            var kValue;
            if ( k in O ) {
                kValue = O[ k ];
                callback.call( T, kValue, k, O );
            }
            k++;
            }
        };
        }
    6、圆角问题：
        z-index:1;
        position: relative;
        border-radius: 50%;
		behavior: url("/enterprise/htc/PIE.htc");
    7、阴影问题
        建议ie8放弃（使用PIE效果太差），改为边框
        border: 1px solid #DCDFE6 \9;
		border-color:#DCDFE6 \9;
		box-sizing: border-box \9;
    8、Object.defineProperty
        <script>
			var origDefineProperty = Object.defineProperty;
			var arePropertyDescriptorsSupported = function () {
				var obj = {};
				try {
					origDefineProperty(obj, "x", {enumerable: false, value: obj});
					for (var _ in obj) {
						return false;
					}
					return obj.x === obj;
				} catch (e) {
					/* this is IE 8. */
					return false;
				}
			};
			var supportsDescriptors = origDefineProperty && arePropertyDescriptorsSupported();
	
			if (!supportsDescriptors) {
				Object.defineProperty = function (a, b, c) {
					//IE8支持修改元素节点的属性
					if (origDefineProperty6 && a.nodeType == 1) {
						return origDefineProperty(a, b, c);
					} else {
						a[b] = c.value || (c.get && c.get());
					}
				};
			}
		</script>
    9、console兼容ie8
        window._console = window.console;//将原始console对象缓存
        window.console = (function (orgConsole) {
            return {//构造的新console对象
                log: getConsoleFn("log"),
                debug: getConsoleFn("debug"),
                info: getConsoleFn("info"),
                warn: getConsoleFn("warn"),
                exception: getConsoleFn("exception"),
                assert: getConsoleFn("assert"),
                dir: getConsoleFn("dir"),
                dirxml: getConsoleFn("dirxml"),
                trace: getConsoleFn("trace"),
                group: getConsoleFn("group"),
                groupCollapsed: getConsoleFn("groupCollapsed"),
                groupEnd: getConsoleFn("groupEnd"),
                profile: getConsoleFn("profile"),
                profileEnd: getConsoleFn("profileEnd"),
                count: getConsoleFn("count"),
                clear: getConsoleFn("clear"),
                time: getConsoleFn("time"),
                timeEnd: getConsoleFn("timeEnd"),
                timeStamp: getConsoleFn("timeStamp"),
                table: getConsoleFn("table"),
                error: getConsoleFn("error"),
                memory: getConsoleFn("memory"),
                markTimeline: getConsoleFn("markTimeline"),
                timeline: getConsoleFn("timeline"),
                timelineEnd: getConsoleFn("timelineEnd")
            };
            function getConsoleFn(name) {
                return function actionConsole() {
                    if (typeof (orgConsole) !== "object") return;
                    if (typeof (orgConsole[name]) !== "function") return;//判断原始console对象中是否含有此方法，若没有则直接返回
                    return orgConsole[name].apply(orgConsole, Array.prototype.slice.call(arguments));//调用原始函数
                };
            }
        }(window._console));
    10，background: rgba(255, 109, 112, 0.06); 兼容
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#19ff6d70,endColorstr=#19ff6d70);
        rgba 和 IE 的 filter数值转换
        0.1     19
        0.2     33
        0.3     4C
        0.4     66
        0.5     7F
        0.6     99
        0.7     B2
        0.8     C8
        0.9     E5
        ff6d70为正常的颜色
    11、background-size:cover 兼容ie8解决方案
        -ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='path_relative_to_the_HTML_file', sizingMethod='scale')";
        （src为图片地址）
        示例 ：
            background-repeat: no-repeat;
            background-image: url(/enterprise/img/beijing@2x.png);
            background-size: cover;
            background-image: none \9;
            background-image: url(/enterprise/img/beijing@2x.png) \9\0;
            -ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='/enterprise/img/beijing@2x.png', sizingMethod='scale')";