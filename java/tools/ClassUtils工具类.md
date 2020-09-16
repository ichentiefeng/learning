

```java
import java.lang.reflect.Field;

/**
 * 类工具类（主要运用反射方法）
 *
 * @author chentiefeng
 * @create 2020-09-16 15:27
 */
public class ClassUtils {


    /**
     * 根据属性名获取该对象属性的字符串值
     * @param fieldName
     * @return
     */
    public static String  getFieldStringValue(Object obj,String fieldName){
        Object fieldValue=getFieldValue(obj,fieldName);
        return fieldValue==null?null:String.valueOf(fieldValue);
    }

    /**
     * 根据属性名获取对象该属性的值
     * @param obj
     * @param fieldName
     * @return
     */
    public static Object  getFieldValue(Object obj,String fieldName){
        Class  clazz=obj.getClass();
        try {
            Field field=clazz.getDeclaredField(fieldName);
            return field.get(obj);
        }catch (Exception e){
            return null;
        }
    }
}
```

