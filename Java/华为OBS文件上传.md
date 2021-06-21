# 华为obs 文件上传

```
package cn.enn.ssd.photovoltaicWeixinService.utils;

import cn.enn.ssd.photovoltaicWeixinService.domain.ResultDTO;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.obs.services.ObsClient;
import com.obs.services.exception.ObsException;
import com.obs.services.model.ObsObject;
import com.obs.services.model.PutObjectResult;
import java.io.BufferedOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URLEncoder;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.commons.lang.StringUtils;
import org.apache.poi.util.IOUtils;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.multipart.MultipartFile;

@Service
public class ObsUtils {
    private static final org.slf4j.Logger log = org.slf4j.LoggerFactory
        .getLogger(ObsUtils.class);

    String AK="LYWFFE1TURIILXM45RSQ";
    String SK="3EMYwui4naZhA5qcUy5yBNGPuWzzh0p7e3HfQKu8";
    String END_POINT="obs.cn-east-3.myhuaweicloud.com";
    String BUCKET_NAME="pro-pv-image";

    public ResultDTO fileUpload( MultipartFile multipartFile) {
        try {
            // 创建ObsClient实例
            ObsClient obsClient = new ObsClient(AK, SK, END_POINT);
            // 上传文件，注意：上传内容大小不能超过5GB
            String objectKey = multipartFile.getOriginalFilename();
            InputStream inputStream = multipartFile.getInputStream();
            PutObjectResult putObjectResult = obsClient.putObject(BUCKET_NAME, objectKey, inputStream);
            String url =  putObjectResult.getObjectUrl();
            JSONObject jsonObject = new JSONObject();
            jsonObject.put("name", objectKey);
            jsonObject.put("url", url);
            inputStream.close();
            obsClient.close();
            return ResultDTO.success(jsonObject);
        } catch (Exception e) {
            log.error("{}文件上传失败！", multipartFile.getOriginalFilename());
            return ResultDTO.fail("B00001","文件上传失败！");
        }
    }


    public ResultDTO fileDownload(HttpServletRequest request, HttpServletResponse response, String fileName) {
        try {
            // 创建ObsClient实例
            ObsClient obsClient = new ObsClient(AK, SK, END_POINT);
            ObsObject obsObject = obsClient.getObject(BUCKET_NAME, fileName);
            InputStream inputStream = obsObject.getObjectContent();
            // 缓冲文件输出流
            BufferedOutputStream outputStream = new BufferedOutputStream(response.getOutputStream());
            // 为防止 文件名出现乱码
            final String userAgent = request.getHeader("USER-AGENT");
            // IE浏览器
            if (StringUtils.contains(userAgent, "MSIE")) {
                fileName = URLEncoder.encode(fileName, "UTF-8");
            } else {
                // google,火狐浏览器
                if (StringUtils.contains(userAgent, "Mozilla")) {
                    fileName = new String(fileName.getBytes(), "ISO8859-1");
                } else {
                    // 其他浏览器
                    fileName = URLEncoder.encode(fileName, "UTF-8");
                }
            }
            response.setContentType("application/x-download");
            // 设置让浏览器弹出下载提示框，而不是直接在浏览器中打开
            response.addHeader("Content-Disposition", "attachment;filename=" + fileName);
            IOUtils.copy(inputStream, outputStream);
            outputStream.flush();
            outputStream.close();
            inputStream.close();
            return null;
        } catch (IOException | ObsException e) {
            log.error("文件下载失败：{}", e.getMessage());
            return ResultDTO.fail("B00001","文件下载失败！");
        }
    }


}
```

##  BUCKET_NAME
可以修改BUCKET_NAME为公开权限，让上传的文件（图片能直接访问）
