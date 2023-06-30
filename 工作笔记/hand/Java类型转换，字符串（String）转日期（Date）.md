Java类型转换，字符串（String）转日期（Date）

----

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateTimeConversion {
    public static void main(String[] args) {
        String dateString = "2011-07-28 00:00:00";

        // 定义输入字符串的日期时间格式
        String inputFormat = "yyyy-MM-dd HH:mm:ss";

        // 定义输出日期时间格式，对应 MySQL 的 datetime 类型
        String outputFormat = "yyyy-MM-dd HH:mm:ss";

        try {
            // 使用 SimpleDateFormat 解析输入字符串为 Date 对象
            SimpleDateFormat inputFormatter = new SimpleDateFormat(inputFormat);
            Date date = inputFormatter.parse(dateString);

            // 使用 SimpleDateFormat 将 Date 对象格式化为输出字符串
            SimpleDateFormat outputFormatter = new SimpleDateFormat(outputFormat);
            String mysqlDatetimeString = outputFormatter.format(date);

            System.out.println("MySQL datetime string: " + mysqlDatetimeString);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}
```

