---
title: OpenLayers 加载 Google 地图
date: 2013-01-22
categories:
- iteye
tags:
- javascript
- gis
- openlayers
---
内容摘要
<!-- more -->

[原文地址](http://fiftyk.iteye.com/admin/blogs/1773520)

原先发在了 OpenLayers 中文网,交流的人很少，所有又发到这里。
之前看了一些继承自 TileCache 的实现,这里也发一下自己的实现,继承自 XYZ 类。接触 OpenLayers 时间不长,没有实际项目应用经验,有理解不清的地方还望各位多多指教,谢谢！

```javascript
OpenLayers.Layer.GMapLayer = OpenLayers.Class(OpenLayers.Layer.XYZ, {
    wrapDateLine:true,
    sphericalMercator:true,
    
    urlTpl:"&hl=zh-CN&gl=CN&src=app&x=${x}&y=${y}&z=${z}",
    
    /**
      * @constructor
      * @param {String} name
      * @param {String} url
      * @param {Object} options
      */
    initialize:function(name,url,options){
            options = OpenLayers.Util.applyDefaults({}, 
              options);
            
            OpenLayers.Layer.XYZ.prototype.initialize.apply(this,
              [name,url,options]);
    },
    
    getURL: function (bounds) {
        var xyz = this.getXYZ(bounds);
        var url = this.url;
        if (OpenLayers.Util.isArray(url)) {
            var s = '' + xyz.x + xyz.y + xyz.z;
            url = this.selectUrl(s, url) + this.urlTpl;
        }
        
        return OpenLayers.String.format(url, xyz);
    },
    
    destory:function(){
        this.sphericalMercator = null;
        this.urlTpl = null;
        OpenLayers.Layer.XYZ.prototype.destroy.apply(this, arguments);
    },

    clone:function(obj){
            if(obj == null){
                    obj = new OpenLayers.Layer.GMapLayer(this.name,this.url,
                      this.getOptions());
            }

            obj = OpenLayers.Layer.XYZ.prototype.clone.apply(this,[obj]);

            return obj;
    },

    getXYZ:function(bounds){
            var res = this.getServerResolution();
    var x = Math.round((bounds.left - this.maxExtent.left) /
        (res * this.tileSize.w));

    var y = Math.round((this.maxExtent.top - bounds.top) /
        (res * this.tileSize.h));

    var z = this.getServerZoom();//继承自Grid

    if (this.wrapDateLine) {
        var limit = Math.pow(2, z);
        x = ((x % limit) + limit) % limit;
    }

    return {'x': x, 'y': y, 'z': z};
    },

    CLASS_NAME: "OpenLayers.Layer.GMapLayer"
});
```

使用方法:

```javascript
var map = new OpenLayers.Map("map",{
    layers:[
        new OpenLayers.Layer.GMapLayer("Google卫星图层",
            ["http://mt1.google.cn/vt/lyrs=s@123"]),
        new OpenLayers.Layer.GMapLayer("Google标注图层",
            ["http://mt2.google.cn/vt/imgtp=png32&lyrs=h@205000000"]),
        new OpenLayers.Layer.GMapLayer("Google矢量图层",
            ["http://mt1.google.cn/vt/lyrs=m@205000000"]),
        new OpenLayers.Layer.GMapLayer("Google地形图层",
            ["http://mt1.google.cn/vt/lyrs=t@130,r@205000000"]),
        new OpenLayers.Layer.GMapLayer("Google路况图层",
            ["http://mt0.google.com/vt?lyrs=m@205000000,traffic|seconds_into_week:-1"])
    ]
    ,center:new OpenLayers.LonLat(120.30549379552448,31.55342767838905),
});
//图层切换控件
map.addControl(new OpenLayers.Control.LayerSwitcher());
//鹰眼控件
map.addControl(new OpenLayers.Control.OverviewMap());
```