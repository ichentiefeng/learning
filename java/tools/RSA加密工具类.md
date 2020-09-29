## RSA加密工具类

```java
import org.bouncycastle.util.encoders.Base64;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import java.security.*;
import java.security.spec.*;
import java.util.HashMap;
import java.util.Map;

/**
 * RSA加密类
 *
 * @author chentiefeng
 * @create 2020-09-27 14:03
 */
public class RSAUtils {

    /** *//**
     * 加密算法RSA
     */
    public static final String KEY_ALGORITHM = "RSA";
    /**
     * 公钥
     */
    private static final String PUBLIC_KEY="RSA_PublicKey";


    /**
     * 私钥
     */
    private static final String PRIVATE_KEY="RSA_PrivateKey";


    /**
     * RSA密钥默认长度
     */
    private static final int KEY_SIZE = 512;

    /**
     * 私钥解密
     * @param data  待解密数据
     * @param key  私钥
     * @return byte[] 解密数据
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     * @throws NoSuchPaddingException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     */
    public static byte[] decryptByPrivateKey(byte[] data,String  key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return decryptByPrivateKey(data,Base64.decode(key));
    }

        /**
         * 私钥解密
         * @param data  待解密数据
         * @param key  私钥
         * @return byte[] 解密数据
         * @throws NoSuchAlgorithmException
         * @throws InvalidKeySpecException
         * @throws NoSuchPaddingException
         * @throws BadPaddingException
         * @throws IllegalBlockSizeException
         */
    public static byte[] decryptByPrivateKey(byte[] data,byte[] key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return crypto(data,key,KeyType.PrivateKey,Cipher.DECRYPT_MODE);
    }


    /**
     * 公钥解密
     * @param data 待解密数据
     * @param key 公钥
     * @return
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     * @throws NoSuchPaddingException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     * @throws InvalidKeyException
     */
    public static byte[] decryptByPublicKey(byte[] data,String key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return decryptByPublicKey(data,Base64.decode(key));
    }
        /**
         * 公钥解密
         * @param data 待解密数据
         * @param key 公钥
         * @return
         * @throws NoSuchAlgorithmException
         * @throws InvalidKeySpecException
         * @throws NoSuchPaddingException
         * @throws BadPaddingException
         * @throws IllegalBlockSizeException
         * @throws InvalidKeyException
         */
    public static byte[] decryptByPublicKey(byte[] data,byte[] key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return crypto(data,key,KeyType.PublicKey,Cipher.DECRYPT_MODE);
    }

    /**
     * 私钥加密
     * @param data  待加密数据
     * @param key  私钥
     * @return byte[] 加密数据
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     * @throws NoSuchPaddingException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     */
    public static byte[] encryptByPrivateKey(byte[] data,String  key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return encryptByPrivateKey(data,Base64.decode(key));
    }
        /**
         * 私钥加密
         * @param data  待加密数据
         * @param key  私钥
         * @return byte[] 加密数据
         * @throws NoSuchAlgorithmException
         * @throws InvalidKeySpecException
         * @throws NoSuchPaddingException
         * @throws BadPaddingException
         * @throws IllegalBlockSizeException
         */
    public static byte[] encryptByPrivateKey(byte[] data,byte[] key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return crypto(data,key,KeyType.PrivateKey,Cipher.ENCRYPT_MODE);
    }

    /**
     * 公钥加密
     * @param data 待加密数据
     * @param key 公钥
     * @return  加密数据
     * @throws NoSuchPaddingException
     * @throws NoSuchAlgorithmException
     * @throws IllegalBlockSizeException
     * @throws BadPaddingException
     * @throws InvalidKeyException
     * @throws InvalidKeySpecException
     */
    public static byte[] encryptByPublicKey(byte[] data,String key) throws NoSuchPaddingException, NoSuchAlgorithmException, IllegalBlockSizeException, BadPaddingException, InvalidKeyException, InvalidKeySpecException {
        return encryptByPublicKey(data,Base64.decode(key));
    }

    /**
     * 公钥加密
     * @param data 待加密数据
     * @param key 公钥
     * @return  加密数据
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     * @throws NoSuchPaddingException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     * @throws InvalidKeyException
     */
    public static byte[] encryptByPublicKey(byte[] data,byte[] key) throws NoSuchAlgorithmException, InvalidKeySpecException, NoSuchPaddingException, BadPaddingException, IllegalBlockSizeException, InvalidKeyException {
        return crypto(data,key,KeyType.PublicKey,Cipher.ENCRYPT_MODE);
    }

    /**
     * 获取私钥
     * @param keyMap
     * @return
     */
    public static  byte[] getPrivateKey(Map<String,Key> keyMap){
        return keyMap.get(PRIVATE_KEY).getEncoded();
    }

    /**
     * 获取私钥字符串
     * @param keyMap
     * @return
     */
    public static  String getPrivateKeyStr(Map<String,Key> keyMap){
        return Base64.toBase64String(getPrivateKey(keyMap));
    }

    /**
     * 获取公钥
     * @param keyMap
     * @return
     */
    public static  byte[] getPublicKey(Map<String,Key> keyMap){
        return keyMap.get(PUBLIC_KEY).getEncoded();
    }

    /**
     * 获取公钥字符串
     * @param keyMap
     * @return
     */
    public static  String getPublicKeyStr(Map<String,Key> keyMap){
        return Base64.toBase64String(getPublicKey(keyMap));
    }

    /**
     * 初始化密钥
     */
    public static Map<String,Key> initKeys() throws NoSuchAlgorithmException {
        //RSA算法要求有一个可信任的随机数源
        SecureRandom secureRandom = new SecureRandom();
        //实例化密钥对生成器
        KeyPairGenerator keyPairGenerator=KeyPairGenerator.getInstance(KEY_ALGORITHM);
        //利用上面的随机数据源初始化这个KeyPairGenerator对象
        keyPairGenerator.initialize(KEY_SIZE, secureRandom);
        //生成密匙对
        KeyPair keyPair = keyPairGenerator.generateKeyPair();
        //公钥
        PublicKey publicKey=keyPair.getPublic();
        //私钥
        PrivateKey privateKey=keyPair.getPrivate();
        //封装密钥
        Map<String,Key> keyMap=new HashMap<>(2);
        keyMap.put(PUBLIC_KEY,publicKey);
        keyMap.put(PRIVATE_KEY,privateKey);
        return keyMap;
    }

    /**
     * 对数据加密或解密
     * @param data 待解密数据
     * @param key 密钥
     * @param keyType 密钥类型
     * @param cipherType 加密或解密，Cipher.DECRYPT_MODE解密、Cipher.ENCRYPT_MODE加密
     * @return
     * @throws NoSuchAlgorithmException
     * @throws NoSuchPaddingException
     * @throws InvalidKeySpecException
     * @throws InvalidKeyException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     */
    private  static byte[] crypto(byte[] data,byte[] key,KeyType keyType,int cipherType) throws NoSuchAlgorithmException, NoSuchPaddingException, InvalidKeySpecException, InvalidKeyException, BadPaddingException, IllegalBlockSizeException {
        EncodedKeySpec encodedKeySpec;
        if(keyType.equals(KeyType.PrivateKey)){
            //取得私钥
            encodedKeySpec=new PKCS8EncodedKeySpec(key);
        }else{
            //取得公钥
            encodedKeySpec=new X509EncodedKeySpec(key);
        }
        KeyFactory keyFactory=KeyFactory.getInstance(KEY_ALGORITHM);
        Key decryptKey;
        if(keyType.equals(KeyType.PrivateKey)){
            //生成私钥
            decryptKey=keyFactory.generatePrivate(encodedKeySpec);
        }else{
            //生成公钥
            decryptKey=keyFactory.generatePublic(encodedKeySpec);
        }
        //对数据操作
        Cipher cipher=Cipher.getInstance(keyFactory.getAlgorithm());
        cipher.init(cipherType,decryptKey);
        return cipher.doFinal(data);
    }

    /**
     * 密钥类型
     *
     * @author Looly
     *
     */
    public enum KeyType {
        /**
         * 公钥
         */
        PublicKey(Cipher.PUBLIC_KEY),
        /**
         * 私钥
         */
        PrivateKey(Cipher.PRIVATE_KEY);

        /**
         * 构造
         *
         * @param value 见{@link Cipher}
         */
        KeyType(int value) {
            this.value = value;
        }

        private final int value;

        /**
         * 获取枚举值对应的int表示
         *
         * @return 枚举值对应的int表示
         */
        public int getValue() {
            return this.value;
        }
    }
}
```

## 测试

```java
    public static void main(String[] args) throws Exception {
        String password="123456";
        Map<String, Key> keyMap= RSAUtils.initKeys();
        //公钥
        String publicKey= RSAUtils.getPublicKeyStr(keyMap);
        //私钥
        String privateKey= RSAUtils.getPrivateKeyStr(keyMap);
        System.out.println(publicKey);
        System.out.println(privateKey);

        //私钥加密
        byte[] encryptData= RSAUtils.encryptByPrivateKey(password.getBytes(),privateKey);
        String encrypt=Base64.toBase64String(encryptData);
        //公钥解密
        byte[] decryptData= RSAUtils.decryptByPublicKey(encryptData,publicKey);
        String decrypt=new String(decryptData,"UTF-8");
        System.out.println(encrypt);
        System.out.println(decrypt);

        //公钥加密
        byte[] encryptData2= RSAUtils.encryptByPublicKey(password.getBytes(),publicKey);
        String encrypt2=Base64.toBase64String(encryptData2);
        //私钥解密
        byte[] decryptData2= RSAUtils.decryptByPrivateKey(encryptData2,privateKey);
        //或者直接解密加密后的字符串内容
        byte[] decryptData3= RSAUtils.decryptByPrivateKey(Base64.decode(encrypt2),privateKey);
        String decrypt2=new String(decryptData2,"UTF-8");
        System.out.println(encrypt2);
        System.out.println(decrypt2);
    }
```



