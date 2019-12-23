## 6.1.1.全部方法     :id=method

方法 | 说明
:--- | :---
void exportData(List<String[]> data, String fileName, HttpServletResponse response) | 导出excel
void exportData(HSSFWorkbook wb, String fileName, HttpServletResponse response) | 导出excel
HSSFWorkbook exportData(List<String[]> data) | 导出excel
 | 
 List<String[]> importData(InputStream inputStream) | 读取excel数据
 List<String[]> importData(InputStream inputStream, int startRow, int startCell) | 读取excel数据
 | 
 List<Map<String, Object>> strListToMap(List<String[]> strList, String[] names) | String数组转Map
 List<T> strListToObject(List<String[]> strList, String[] names, Class<T> classz) | String数组转对象
 | 
 String getCellValue(Cell cell) | 读取单元格的值


## 6.1.1.使用示例     :id=demo
```java
public class DemoController {
    public void demo(HttpServletRequest request, HttpServletResponse response, MultipartFile file){
        // 导出数据 
        List<String[]> exportList = new ArrayList<>();
        exportList.add(new String[]{"姓名", "性别", "年龄"});
        exportList.add(new String[]{"userA", "male", "12"});
        exportList.add(new String[]{"userB", "male", "22"});
        PoiUtil.exportData(exportList, "用户列表", response);
        
        // 导入数据
        List<String[]> list = PoiUtil.importData(file.getInputStream());
        
        // 第一行不导入
        List<String[]> list = PoiUtil.importData(file.getInputStream(), 1, 0);
        
        // 第一行第一列不导入
        List<String[]> list = PoiUtil.importData(file.getInputStream(), 1, 1);
        
        // 将导入的String数组转成User对象
        List<User> userList = PoiUtil.strListToObject(list, new String[]{"name", "sex", "age"}, User.class);
        // 参数二是数组的每一个位置对应的对象的字段名
    }
}
```
