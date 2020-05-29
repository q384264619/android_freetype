# 项目说明
    在android中使用CMakeLists.txt编译生成静态库或So动态库
    
# 工程中已经写好了Android.mk与CMakeLists.txt
    你只用将该工程文件全部拷到你的工程的main下的cpp目录中，然后在gradle中配制下:
 
 > 在default中添加：
 
 ```
     externalNativeBuild {
            cmake {
                cppFlags ""
                abiFilters "armeabi-v7a"//"arm64-v8a"
            }
        }
  ```
  > 在default外添加：
    
 ```   
       externalNativeBuild {
          cmake {
              path "src/main/cpp/CMakeLists.txt"
          }
      }
      
  ```  
# 代码调用例子：

```c++
    #include <freetype/ftglyph.h>
    #include FT_TRUETYPE_IDS_H
```

```c++
      char16_t* osd = "A";
    int len = 1;
      for ( n = 0; n < len; n++ )
    {
        LOGE("load char 0x%02x",(osd[n]));
        FT_Library  library = NULL;   /* handle to library     */
        FT_Face     fface =NULL ;      /* handle to face object */

        #加载free库
        error = FT_Init_FreeType(&library);

        if(error){
            LOGE("init freetype error");
            continue;
        }
    //    error = FT_New_Face(library,"/system/fonts/DroidSans.ttf",0,&fface);
      //如果小于127则认为是ascii单字节，我们可以加载没有中文的字体，否则加载中文字体
        if(osd[n]>127){
            error = FT_New_Face(library,"/system/fonts/DroidSansChinese.ttf",0,&fface);
        }
        else{
            error = FT_New_Face(library,"/system/fonts/DroidSans.ttf",0,&fface);
        }



        if(error){
            LOGE("init face error");
            continue;
        }
    //选择字符集，我们现在选择unicode,表示将来去查找字形图片都是根据unicode编码去查找
        error = FT_Select_Charmap(fface,FT_ENCODING_UNICODE);

        if(error){
            LOGE("init select charmap error");
            continue;
        }
  //设置生成的字符图像大小
        FT_Set_Pixel_Sizes(fface,30,30);

        FT_GlyphSlot slop = fface->glyph;
//加载，并绘制到内存中
        error= FT_Load_Char(fface, osd[n],  FT_LOAD_RENDER);

        if ( error )
        {
            LOGE(" FT_Load_Char  error");
            continue;  /* ignore errors */
        }

       FT_Bitmap* bitmap = &slop->bitmap;

        LOGE("left:%d,top:%d, lsb_delta:%d ,rsb:%d,",(int)slop->bitmap_left,(int)slop->bitmap_top,slop->lsb_delta,slop->rsb_delta);

        int w  = bitmap->width;
        int h  = bitmap->rows;
//
//        switch (bitmap->pixel_mode)
//        {
//            case FT_PIXEL_MODE_BGRA:
//            {
//                LOGE("FT_PIXEL_MODE_BGRA");
//                break;
//            }
//            case FT_PIXEL_MODE_GRAY:
//            {
//                LOGE("FT_PIXEL_MODE_GRAY");
//                break;
//            }
//            case FT_PIXEL_MODE_MONO:
//            {
//                LOGE("FT_PIXEL_MODE_MONO");
//                break;
//            }
//            case FT_PIXEL_MODE_NONE:
//            {
//                LOGE("FT_PIXEL_MODE_NONE");
//                break;
//            }
//            default:
//            {
//                LOGE("other mode:%d",bitmap->pixel_mode);
//            }
//        }


        uint8 * osdYuv = static_cast<uint8 *>(malloc(w * h * 3 / 2));
// 将rgb数据拷到yuv内存中，只用的了Y分量
        memcpy(osdYuv,bitmap->buffer,w*h);


       //  OsdInfo* info = new OsdInfo((int)slop->bitmap_left,(int)slop->bitmap_top, w, h, osdYuv);



        this->osdList.push_back(info);


        if(fface){
            FT_Done_Face(fface);
        }
        fface = NULL;

        if(library){
            FT_Done_FreeType(library);
        }
        library = NULL;
    }
    
```    
    
