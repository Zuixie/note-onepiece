## 1. del amazon kindle book remote with javascript code
```javascript
function test() {
    for (var i = 0; i < 10; i++){
        var chk = document.getElementById("chk"+i);
        if (chk != null) {
            chk.click();
        }
    }
    document.getElementById("contentAction_delete_myx").click();
    document.getElementById("dialogButton_ok_myx").click();
    setTimeout(function(){
        var okbtn = document.getElementById("dialogButton_ok_myx");
        if (okbtn != null) {
            okbtn.click();
            setTimeout(test(), 2000);
        }
    },5000);
}    
```

## 2. xdg-open
```os.system("xdg-open %s &" % image_name )```类似双击操作，使用系统的默认程序打开指定文件

