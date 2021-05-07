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
    