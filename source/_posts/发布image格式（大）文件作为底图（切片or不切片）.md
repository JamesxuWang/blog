---
title: 发布image格式（大）文件作为底图（切片or不切片）
categories:
  - webgis
date: 2018-11-19 18:25:37
tags: [前端, GIS]
---

公司项目需要发布一张工程CAD底图，考虑到成本和效率，不考虑arcgisServer，然后就用geoserver进行发布服务的过程中发现了其他各种方案。进而打算找处理软件来对源图片直接进行切片放在服务器上自定义的XYZ加载，但是找了许多天都没有找到简单好用的一键切片及配准软件工具。所以自己总结了一下网上找的几种方案。

*   deep zoom composer（切片太简陋有点摸不着头脑）
*   GMapImageCutter  切片的格式比较奇怪搞不定（谷歌地球专用？）
*   MapTiler  切片简单好用带demo，但是不付费有水印
*   MapCruncher 这个话，测试发现2.3.1版已经打不开了（我也想要个能用的）
*   GDAL工具切片（直接PYTHON調用或是在QGIS 使用）（最友善）
*   geoserver直接发布worldImage 文件（小文件可行）（不需要切片直接发布，最好有坐标配置文件）（最便捷）

**总结了其中的两种思路，其他详细步骤在网上可以查到：** **第一种**情况是需要与现实地图重合配准的，所以就打算重拾老本行，用arcgis处理成tif格式，配准后导出geotiff使用geoserver发布,

1.  打开arcmap加载具有参考坐标的地图和需要发布的来源图片
2.  打开地理配准工具条选择自定义-工具条中的地理配准（Georeference）并勾选
3.  选择控制点（大于3个，建议7个）（注意可以设置图层透明度查看匹配程度）然后校正保存
4.  重新加载保存的数据（建议重启arcmap），打开arcToolBox，在**转换工具**中找到**转为栅格**中的**栅格转其他类型（批量）**，导出成带有坐标的Geotiff.
5.  geoserver 发布geotiff
6.  ol加载geoserver~ImageWMS

**第二种**是不要配准的，类似旅游一张图，或者是工程坐标图（CAD设计图）之类的，我使用NodeJS+ express 写了个上传调用GDAL-python中的gdal2tile.py的脚本自动切片。 安装python工具和安装GDAL软件库（注意版本一定要对应上，3.4的和3.7的完全不一样）、（GDAL-2.2.4-cp37-cp37m-win_amd64）+（python-3.7.0-amd64） 检查安装环境（PYTHON、PYTHON/Scripts及GDAL的文件夹路径放入PATH）, 直接切图 

`gdal2tiles.py [-p profile] [-r resampling] [-s srs] [-z zoom] [-e] [-a nodata] [-v] [-q] [-h] [-k] [-n] [-u url] [-w webviewer] [-t title] [-c copyright] [--processes=NB_PROCESSES] [-g googlekey] [-b bingkey] input_file [output_dir]`

 gdal2tiles.py -p mercator -z 切片级别 -w 查看的平台 -t 名称 -r 重采样方案 -s EPSG:4326 -a 0.0 输入文件路径.jpg 输出文件路径 例子：（gdal2tiles.py -p raster -z 3-6 -w all -t 123 -c 1 -r average -a 0.0 D:/arcgisData/gis/7891.png D:\\arcgisData\\gis\\20180824） 简单的node的后台代码（需要配node环境）

```javascript
var fs = require('fs');
var express = require('express');
var multer  = require('multer');
var nodeCmd = require('node-cmd');
var router = express.Router();
var upload = multer({dest: 'public/upload_temp/'});
var exec = require('child_process').exec;
router.post('/', upload.any(), function(req, res, next) {
    console.log(req.files\[0\]);  // 上传的文件信息
    var des_file = "public/acture/" + req.files\[0\].originalname;
    fs.readFile( req.files\[0\].path, function (err, data) {
        fs.writeFile(des_file, data, function (err) {
            if( err ){
                console.log( err );
                res.end( JSON.stringify( err ) );
            }else{
                response = {
                    message:'File uploaded successfully',
                    filename:req.files\[0\].originalname
                };
                runCmdTest(des_file);
                console.log( response );
                res.end( JSON.stringify( response ) );

            }
        });
    });

});
function runCmdTest(abc) {
    var host_dir = 'public/dataForTile/adc';
    var path = 'gdal2tiles.py -p raster -z 3-6 -w all -t 1 -c 1 -r average -a 0.0 '+abc+' '+host_dir;
    nodeCmd.get(
        path,
        function(err, data, stderr){
            if(err){
                console.log(err);
            }else{
                console.log(data);
            }
        }
    );
}
module.exports = router;
```

把切出后的整个文件夹放在IIS或是tomcat下面，然后使用TileImage调用即可~ openlayer5核心加载代码

```
source : new ol.source.TileImage({
    wrapX:false,
    crossOrigin: 'anonymous',
    tileUrlFunction : function(tileCoord){
        if (!tileCoord) {
            return "";
        }
        var z = tileCoord\[0\];
        var x = tileCoord\[1\];
        var y = Math.pow(2, z-1) + tileCoord\[2\];
        var path = res.CadFilePath +  "/" + z + "/" + x + "/" + y + ".png";
        // 非常粗糙，需要验证
        if((z==3&&x<=4&&x>=0&&y<=3&&y>=0)||(z==4&&x<=9&&x>=0&&y<=6&&y>=0)||(z==5&&x<=18&&x>=0&&y<=12&&y>=0)||(z==6&&x<=36&&x>=0&&y<=25&&y>=0)){
            return path;
        }else{
            return '../../../images/none.png';     // var emptyTileURL = "http://www.maptiler.org/img/none.png";
        }
    }
})
```

知识点不多，跨度有点大,有点杂。放后台切片代码凑字数哈哈哈哈啊