# 若依Vue前后端分离版本改SM3加密

最近因为在做一个政府项目，要求使用SM3加密，先将用到的若依框架前后端分离版本登录加密修改为SM3加密

若依是个很好的脚手架，单体版若依使用shiro实现安全认证与权限控制，而分离版使用的是Spring Security实现，我们阅读若依源码时可以发现，若依的framework模块下有个security包，这里面是若依自己封装的认证失败处理类等，与加密登录验证无关，跳过。我们需要关注的是framework模块下的config包下的SecurityConfig类，里面有两个方法：

```
    /**
     * 强散列哈希加密实现
     */
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder()
    {
        return new BCryptPasswordEncoder();
    }

    /**
     * 身份认证接口
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception
    {
        auth.userDetailsService(userDetailsService).passwordEncoder(bCryptPasswordEncoder());
    }

```

Spring Security通过passwordEncoder()进行身份认证，参数为PasswordEncoder接口类，也就是说传入的参数是需要实现PasswordEncoder接口的类，BCryptPasswordEncoder类是强散列哈希类，关于这个类的说明我们在此不介绍，它是一种哈希算法


自定义一个SM3PasswordEncoder，使用SM3加密算法。

```
    /**
     * 强散列哈希加密实现X
     * 修改为SM3杂凑算法加密
     */
    @Bean
    public SM3PasswordEncoder sm3PasswordEncoder()
    {
        return new SM3PasswordEncoder();
    }

    /**
     * 身份认证接口
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception
    {
        auth.userDetailsService(userDetailsService).passwordEncoder(sm3PasswordEncoder());
    }
```

在common模块下新建一个类SM3PasswordEncoder并实现PasswordEncoder接口

```
package com.pro.common.utils.sign;

import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @description:
 * @author: yeats
 * @create_date 2022年04月21日 18:41
 */
public class SM3PasswordEncoder implements PasswordEncoder {
  @Override
  public String encode(CharSequence rawPassword) {
    return SM3.encrypt(rawPassword.toString());
  }

  @Override
  public boolean matches(CharSequence rawPassword, String encodedPassword) {
    return SM3.vertify(rawPassword.toString(), encodedPassword);
  }

}

```

以下是SM3算法实现类：

```
package com.pro.common.utils.sign;
import java.io.UnsupportedEncodingException;
import java.security.Security;
import java.util.Arrays;

import org.bouncycastle.crypto.digests.SM3Digest;
import org.bouncycastle.crypto.macs.HMac;
import org.bouncycastle.crypto.params.KeyParameter;
import org.bouncycastle.jce.provider.BouncyCastleProvider;
import org.bouncycastle.pqc.math.linearalgebra.ByteUtils;

public class SM3 {

  private static final String ENCODING = "UTF-8";

  static {
    Security.addProvider(new BouncyCastleProvider());
  }
  /**
   * SM3加密
   * @param paramStr	待加密字符串
   * @return	返回加密后，固定长度=32的16进制字符串
   */
  public static String encrypt(String paramStr) {
    //将返回的hash值转换为16进制字符串
    String resultHexString = "";
    try {
      //将字符串转换成byte数组
      byte[]  srcData = paramStr.getBytes(ENCODING);
      byte[] resultHash = hash(srcData);
      //将返回的hash值转换成16进制字符串
      resultHexString = ByteUtils.toHexString(resultHash);
    } catch (UnsupportedEncodingException e) {
      e.printStackTrace();
    }
    return resultHexString;
  }
  /**
   * 返回长度为32的byte数组
   * 生成对应的hash值
   * @param srcData
   * @return
   */
  public static byte[] hash(byte[] srcData){
    SM3Digest digest = new SM3Digest();
    digest.update(srcData,0,srcData.length);
    byte[] hash = new byte[digest.getDigestSize()];
    digest.doFinal(hash, 0);
    return hash;
  }
  /**
   * 通过指定密钥进行加密
   * @param key 密钥
   * @param srcData 被加密的byte数组
   * @return
   */
  public static byte[] hmac(byte[] key,byte[] srcData){
    KeyParameter keyParameter = new KeyParameter(key);
    SM3Digest digest = new SM3Digest();
    HMac mac = new HMac(digest);
    mac.init(keyParameter);
    mac.update(srcData,0,srcData.length);
    byte[] result = new byte[mac.getMacSize()];
    mac.doFinal(result, 0);
    return result;
  }
  /**
   * 判断数据源与加密数据是否一致，通过验证原数组和生成是hash数组是否为同一数组，验证二者是否为同一数据
   * @param srcStr
   * @param sm3HexString
   * @return
   */
  public static boolean vertify(String srcStr,String sm3HexString){
    boolean flag = false;
    try {
      byte[] srcData = srcStr.getBytes(ENCODING);
      byte[] sm3Hash = ByteUtils.fromHexString(sm3HexString);
      byte[] newHash = hash(srcData);
      if(Arrays.equals(newHash, sm3Hash));
      flag = true;
    } catch (UnsupportedEncodingException e) {
      e.printStackTrace();
    }
    return flag;
  }

  public static void main(String[] args){
    //测试
    String str = "123";
    String hex = SM3.encrypt(str);
    System.out.println(hex);

    String str1 = "abc123";
    String hex1 = SM3.encrypt(str1);
    System.out.println(hex1);

    String str2 = "qwer12345";
    String hex2 = SM3.encrypt(str2);
    System.out.println(hex2);

    String strpre = "412727";
    String hexpre = SM3.encrypt(strpre);
    System.out.println(hexpre);
    //验证加密后的16进制字符串与加密前的字符串是否相同
    boolean flag = SM3.vertify(str, hex);
    System.out.println(flag);
  }
}

```


好了，到这我们实现了登录的密码验证以sm3的方式验证，但还有一步，那就是加密，比如注册账户、修改密码的时候，我们需要将密码加密以后存入数据库，若依的加密方法在common模块的SecurityUtils类中,我们将原来的方法注释掉，替换为使用sm3加密

```
package com.pro.common.utils;

import com.pro.common.utils.sign.SM3;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import com.pro.common.constant.HttpStatus;
import com.pro.common.core.domain.model.LoginUser;
import com.pro.common.exception.CustomException;

/**
 * 安全服务工具类
 * 
 * @author pro
 */
public class SecurityUtils
{
    /**
     * 获取用户账户
     **/
    public static String getUsername()
    {
        try
        {
            return getLoginUser().getUsername();
        }
        catch (Exception e)
        {
            throw new CustomException("获取用户账户异常", HttpStatus.UNAUTHORIZED);
        }
    }

    /**
     * 获取用户
     **/
    public static LoginUser getLoginUser()
    {
        try
        {
            return (LoginUser) getAuthentication().getPrincipal();
        }
        catch (Exception e)
        {
            throw new CustomException("获取用户信息异常", HttpStatus.UNAUTHORIZED);
        }
    }

    /**
     * 获取Authentication
     */
    public static Authentication getAuthentication()
    {
        return SecurityContextHolder.getContext().getAuthentication();
    }

    /**
     * 生成BCryptPasswordEncoder密码
     *
     * @param password 密码
     * @return 加密字符串
     */
    public static String encryptPassword(String password)
    {
//        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        //2022-04-21 加密算法改为SM3杂凑算法

        return SM3.encrypt(password);
    }

    /**
     * 判断密码是否相同
     *
     * @param rawPassword 真实密码
     * @param encodedPassword 加密后字符
     * @return 结果
     */
    public static boolean matchesPassword(String rawPassword, String encodedPassword)
    {
//        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        //2022-04-21 加密算法改为SM3杂凑算法
        return SM3.vertify(rawPassword, encodedPassword);
    }

    /**
     * 是否为管理员
     * 
     * @param userId 用户ID
     * @return 结果
     */
    public static boolean isAdmin(Long userId)
    {
        return userId != null && 1L == userId;
    }
}

```








[参考原文链接](https://blog.csdn.net/qq_42351346/article/details/123427762)
