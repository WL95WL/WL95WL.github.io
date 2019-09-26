---
title: Base64Utils
date: 2019-09-26 09:23:29
tags: java
---

````java
/**
 * 把七牛云图片转换为base64
 * @author wl
 */
public class NetToBase64 {
    /**
     * 通过图片的url获取图片的base64字符串
     * @param imgUrl    图片url
     * @return    返回图片base64的字符串
     */
    public static String image2Base64(String imgUrl) {

        URL url = null;

        InputStream is = null;

        ByteArrayOutputStream outStream = null;

        HttpURLConnection httpUrl = null;

        try{

            url = new URL(imgUrl);

            httpUrl = (HttpURLConnection) url.openConnection();

            httpUrl.connect();

            httpUrl.getInputStream();

            is = httpUrl.getInputStream();



            outStream = new ByteArrayOutputStream();

            //创建一个Buffer字符串

            byte[] buffer = new byte[1024];

            //每次读取的字符串长度，如果为-1，代表全部读取完毕

            int len = 0;

            //使用一个输入流从buffer里把数据读取出来

            while( (len=is.read(buffer)) != -1 ){

                //用输出流往buffer里写入数据，中间参数代表从哪个位置开始读，len代表读取的长度

                outStream.write(buffer, 0, len);

            }

            // 对字节数组Base64编码

            return Base64Util.encode(outStream.toByteArray());

        }catch (Exception e) {

            e.printStackTrace();

        }

        finally{

            if(is != null)

            {

                try {

                    is.close();

                } catch (IOException e) {

                    e.printStackTrace();

                }

            }

            if(outStream != null)

            {

                try {

                    outStream.close();

                } catch (IOException e) {

                    e.printStackTrace();

                }

            }

            if(httpUrl != null)

            {
                httpUrl.disconnect();

            }
        }
        return imgUrl;

    }

    /**
     * base64 to image
     * @param BASE64str bas64字符串
     * @param path 存储地址
     * @param ext 图片后缀
     * @return 存储地址
     */
    public static String BASE64CodeToBeImage(String BASE64str,String path,String ext){
        File fileDir = new File(path);
        if (!fileDir.exists()) {
            fileDir.setWritable(true);
            fileDir.mkdirs();
        }

        //文件名称
        String uploadFileName =new Date().getTime()+ "."+ext;
        File targetFile = new File(path, uploadFileName);
        BASE64Decoder decoder = new BASE64Decoder();
        try(OutputStream out = new FileOutputStream(targetFile)){
            byte[] b = decoder.decodeBuffer(BASE64str);
            for (int i = 0; i <b.length ; i++) {
                if (b[i] <0) {
                    b[i]+=256;
                }
            }
            out.write(b);
            out.flush();
            return  path+"/"+uploadFileName+"."+ext;
        }catch (Exception e){
            e.printStackTrace();
            return null;
        }
    }

}
 
````



````java
public class Base64Util {
    /**
     14      * 字符串转图片
     15      * @param base64Str
     16      * @return
     17      */
     public static byte[] decode(String base64Str){
                 byte[] b = null;
                 BASE64Decoder decoder = new BASE64Decoder();
                 try {
                         b = decoder.decodeBuffer(replaceEnter(base64Str));
                     } catch (IOException e) {
                         e.printStackTrace();
                     }
                return b;
           }

             /**
 30      * 图片转字符串
 31      * @param image
 32      * @return
 33      */
             public static String encode(byte[] image){
            BASE64Encoder decoder = new BASE64Encoder();
                 return replaceEnter(decoder.encode(image));
            }

             public static String encode(String uri){
                 BASE64Encoder encoder = new BASE64Encoder();
                 return replaceEnter(encoder.encode(uri.getBytes()));
             }

            /**
 45      *
 46      * @path    图片路径
 47      * @return
 48      */

            public static byte[] imageTobyte(String path){
                 byte[] data = null;
                 FileImageInputStream input = null;
               try {
                        input = new FileImageInputStream(new File(path));
                         ByteArrayOutputStream output = new ByteArrayOutputStream();
            byte[] buf = new byte[1024];
                        int numBytesRead = 0;
                        while((numBytesRead = input.read(buf)) != -1){
                                output.write(buf, 0, numBytesRead);
                            }
                        data = output.toByteArray();
                        output.close();
                        input.close();

                     } catch (Exception e) {
                        e.printStackTrace();
        }

            return data;
            }

             public static String replaceEnter(String str){
                 String reg ="[\n-\r]";
                 Pattern p = Pattern.compile(reg);
                Matcher m = p.matcher(str);
                return m.replaceAll("");
             }
}
````

