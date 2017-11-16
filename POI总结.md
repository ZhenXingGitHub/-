## POI总结 ##
### 1、关于POI ###
	POI是很强大的Offer操作组件，可以提供对excel的读写	
### 2、常用对象###
	POIFSFileSystem fs=newPOIFSFileSystem(new FileInputStream("d:/test.xls"));  //建立输出流
	HSSFWorkbook wb = new HSSFWorkbook(fs);  //得到excel工作流对象
	HSSFSheet sheet = wb.getSheetAt(0);   // 得到一个sheet 及工作表
	HSSFSheet sheet = wb.createSheet();//创建一个sheet
 	其他 ： Row 、 Cell、 CellStyle等，CellStyle可设置格式等style
详情API[https://www.cnblogs.com/fqfanqi/p/6172223.html](https://www.cnblogs.com/fqfanqi/p/6172223.html "详情API链接")
###  ###