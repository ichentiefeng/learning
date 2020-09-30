## Token工具类

``` java
/**
 * Token工具类
 *
 * @author chentiefeng
 * @date 2020-09-28 18:20
 */
public class TokenUtils {
    /**
     * 根据用户名生成token
     * @param username 用户名
     * @return token
     */
    public static String getToken(String username) {
        //TODO 按照规则生成
        //TODO 放到缓存中
        return username+"_test";
    }

    /**
     * 验证用户的Token
     * @param username 用户名
     * @return token
     */
    public static boolean checkToken(String username) {
        //TODO 从缓存中取出token验证
        return true;
    }
}
```

