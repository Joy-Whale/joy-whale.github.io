---
title: Gradle之使用BuildConfig自定义常量
date: 2017-02-10 16:06:00
categories:
- 技术
- Android
tags: 
- Android
- Gradle
---
## 正文
&#8195;&#8195;在项目开发中，我们经常会用到网络访问来连接我们自己的服务器，在开发中与正式环境中往往用到不同的接口地址，正常情况下，我们会在代码中自定义一个String常量来存储HTTP头地址，在开发阶段使用测试地址，上线打包时更换为正式地址。而用Android Studio开发项目时，我们可以用更优雅的方式来解决这个问题，即在BuildConfig中来自定义这个接口地址。
&#8195;&#8195;BuildConfig是android studio在打包时自动生成的一个java类，在项目工程的build/generated/source/buildConfig目录下，打开这个目录可以发现会有多个不同的目录来存放BuildConfig.java类，一般会有androidTest、debug、release等多个目录，这些目录中的BuildConfig类中有相同的常量字段，但这里常量字段的值是完全可以自定义的，这样我们就可以通过定义一些常量使其在debug以及release中生成不同的字段，这里我们来定义一个HTTP_BASE字段来使其在debug中使用测试地址而在release中使用正式地址。
在build.gradle中的buildTypes下，我们可以为release以及debug定义我们所需要的常量：
```
buildTypes {  
        release {  
            minifyEnabled true  
            signingConfig signingConfigs.release  
            proguardFiles getDefaultProguardFile('proguard-project.txt'), 'proguard-rules.pro'  
            buildConfigField("String", "HTTP_BASE", '"https://www.baidu.com/api/release/"')  
        }  
  
        debug {  
            signingConfig signingConfigs.debug  
            proguardFiles getDefaultProguardFile('proguard-project.txt'), 'proguard-rules.pro'  
            buildConfigField("String", "HTTP_BASE", '"https://www.baidu.com/api/debug"')  
        }  
}  
```
如上所示，我们可以使用buildConfigField()方法来自定义一些 常量，进入该方法源码
```
/** 
 * Adds a new field to the generated BuildConfig class. 
 * 
 * <p>The field is generated as: <code><type> <name> = <value>;</code> 
 * 
 * <p>This means each of these must have valid Java content. If the type is a String, then the 
 * value should include quotes. 
 * 
 * @param type the type of the field 
 * @param name the name of the field 
 * @param value the value of the field 
 */  
public void buildConfigField(  
        @NonNull String type,  
        @NonNull String name,  
        @NonNull String value) {  
    ClassField alreadyPresent = getBuildConfigFields().get(name);  
    if (alreadyPresent != null) {  
        logger.info("BuildType({}): buildConfigField '{}' value is being replaced: {} -> {}",  
                getName(), name, alreadyPresent.getValue(), value);  
    }  
    addBuildConfigField(AndroidBuilder.createClassField(type, name, value));  
}  
```
使用时只需要调用BuildConfig类的对应静态常量即可