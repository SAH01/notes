# Java封装xml格式参数请求第三方接口

-----

## 1、引用包

```java
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.StringWriter;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
```

## 2、封装方法生成xml格式文本

里面用到了两个方法

1、getStringFromDocument，从Document对象转换为String字符串返回。

2、createElementWithValue，给某个节点创建子节点并赋值。

这是我写的例子和模板样例

```java
public String getXmlParam(BpMaster bpMaster) {
        String RawXml = null;   // 返回值
        try {
            // 创建xml对象
            DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
            Document doc = docBuilder.newDocument();

            // 创建根节点
            Element ufinterface = doc.createElement("ufinterface");
            ufinterface.setAttribute("account", "develop");	// 设置属性
            ufinterface.setAttribute("billtype", "customer");
            doc.appendChild(ufinterface);
            // 创建 bill节点
            Element bill = doc.createElement("bill");
            ufinterface.appendChild(bill);  // 添加bill节点
            bill.setAttribute("id", "");	// 设置属性
            // 创建 billhead
            Element billhead = doc.createElement("billhead");
            bill.appendChild(billhead);	// 添加 billhead 到 bill下
            // 添加billhead子节点和text值
            createElementWithValue(doc, billhead, "pk_group", String.valueOf(bpMaster.getParentCompanyId()));
            // 规范化XML文档
            doc.getDocumentElement().normalize();
            // 获取XML原文
            RawXml = getStringFromDocument(doc);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return RawXml;
    }
```

```java
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class XMLGenerator {
    public static void main(String[] args) {
        try {
            // Create the XML document
            DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
            Document doc = docBuilder.newDocument();

            // Create the root element
            Element ufinterface = doc.createElement("ufinterface");
            ufinterface.setAttribute("account", "develop");
            ufinterface.setAttribute("billtype", "customer");
            ufinterface.setAttribute("isexchange", "Y");
            ufinterface.setAttribute("sender", "sys001");
            doc.appendChild(ufinterface);

            // Create the bill element
            Element bill = doc.createElement("bill");
            ufinterface.appendChild(bill);

            // Create the billhead element
            Element billhead = doc.createElement("billhead");
            bill.appendChild(billhead);

            // Add the elements and their values
            createElementWithValue(doc, billhead, "pk_group", "uap60");
            createElementWithValue(doc, billhead, "pk_org", "uap60");
            createElementWithValue(doc, billhead, "code", "customer_pfxx");
            createElementWithValue(doc, billhead, "name", "customer_pfxx");
            // Add other elements and their values...

            // Output the XML document
            doc.getDocumentElement().normalize();
            System.out.println(getStringFromDocument(doc));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void createElementWithValue(Document doc, Element parent, String tagName, String value) {
        Element element = doc.createElement(tagName);
        element.appendChild(doc.createTextNode(value));
        parent.appendChild(element);
    }

    private static String getStringFromDocument(Document doc) {
        try {
            StringWriter writer = new StringWriter();
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            transformer.setOutputProperty(OutputKeys.OMIT_XML_DECLARATION, "no");
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            transformer.transform(new DOMSource(doc), new StreamResult(writer));
            return writer.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

## 3、getStringFromDocument

```java
/**
     * @description: 转换Document对象为String并返回
     * @author: chuanwei.yang 42624
     * @date: 2023/6/26 15:10
     * @param: doc
     * @return: * @return: java.lang.String
     **/
    private String getStringFromDocument(Document doc) {
        try {
            // 接收转换后的字符串结果
            StringWriter writer = new StringWriter();
            // 生成进行XML文档转换对象
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            // 不省略XML声明
            transformer.setOutputProperty(OutputKeys.OMIT_XML_DECLARATION, "no");
            // 对输出结果进行缩进处理
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            // 将Document对象表示的XML文档内容转换为字符串，并写入到StringWriter对象中
            transformer.transform(new DOMSource(doc), new StreamResult(writer));
            // 将转换后的XML内容作为字符串返回
            return writer.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
```

## 4、createElementWithValue

```java
/**
 * @description: 构建XML文件使用，创建子节点和值。
 * @author: chuanwei.yang 42624
 * @date: 2023/6/26 15:06
 * @param: doc
 * @param: parent
 * @param: tagName
 * @param: value
 * @return: * @return: void
 **/
private void createElementWithValue(Document doc, Element parent, String tagName, String value) {
    Element element = doc.createElement(tagName);
    element.appendChild(doc.createTextNode(value));
    parent.appendChild(element);
}
```

## 5、发起请求

拼接xml格式的参数请求接口。

RawXml变量 是生成的xml参数文本。

```java
            String RawXml = getStringFromDocument(doc);	// 调用方法生成xml文本
			String apiUrl = "接口地址";
            try {
                // 创建URL对象和HttpURLConnection连接
                URL url = new URL(apiUrl);
                HttpURLConnection connection = (HttpURLConnection) url.openConnection();

                // 设置请求方法为POST
                connection.setRequestMethod("POST");

                // 设置请求头信息
                connection.setRequestProperty("Content-Type", "application/xml");

                // 启用输出流，并写入XML数据
                connection.setDoOutput(true);
                OutputStream outputStream = connection.getOutputStream();
                outputStream.write(RawXml.getBytes(StandardCharsets.UTF_8));
                outputStream.close();

                // 发送请求并获取响应
                int responseCode = connection.getResponseCode();    // 获取响应码
                // 获取响应报文
                BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                String line;
                StringBuilder response = new StringBuilder();
                while ((line = reader.readLine()) != null) {
                    response.append(line);
                }
                reader.close();
                // 输出响应结果
                System.out.println("Response Code: " + responseCode);
                System.out.println("Response Body: " + response.toString());
            } catch (Exception e) {
                e.printStackTrace();
            }
```

## 6、实现效果

最终生成的xml文件如下：

![image-20230626170631430](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230626170631430.png)