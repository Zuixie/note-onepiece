# Struts 1
## configure


## 下载文件
```Java
response.setContentType(request.getServletContext().getMimeType(filename));  
//设置Content-Disposition  
response.setHeader("Content-Disposition", "attachment;filename="+filename);  
// outputStream.write()
return null;
```