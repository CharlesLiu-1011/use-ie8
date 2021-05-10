# use-ie8
兼容ie8

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
    4、echarts层级问题，给外层父级元素设置position:relative;z-index:1;(将父级元素设置为定位元素，然后设置层级)
    5、forEach()  ie8  兼容
        if ( √√ ) {
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
        建议ie8放弃（效果太差），改为边框
        border-top: 1px solid #DCDFE6 \9;
		border-color:#DCDFE6 \9;
		box-sizing: content-box \9;
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
					if (origDefineProperty && a.nodeType == 1) {
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
