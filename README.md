

##命令：

gradle assemble<Variant Name>    
allows directly building a single variant. For instance assembleFlavor1Debug.

gradle assemble<Build Type Name>
allows building all APKs for a given Build Type. For instance assembleDebug will build both Flavor1Debug and Flavor2Debug variants.
   
gradle assemble<Product Flavor Name>    
allows building all APKs for a given flavor. For instance assembleFlavor1 will build both Flavor1Debug and Flavor1Release variants.

gradle build：编译整个项目，它会执行代码编译、代码检测和单元测试等

gradle assemble：编译并打包你的代码, 但是并不运行代码检测和单元测试

gradle clean：删除 build 生成的目录和所有生成的文件

gradle check：编译并测试你的代码。其它的插件会加入更多的检查步骤，如使用 checkstyle、pmd、findbugs


##介绍：
**<<** 就等价于 doLast，doLast 是gradle提供访问task任务的一个API，类似的还有 doFirst，当一个task被执行的时候，可以通过 doFirst 和 doLast 向task中动态添加操作。doFirst 和 doLast 会在task本身被执行之后才会被执行。   

###自定义字段
可以通过增加main同级的文件夹来控制不同的productFlavors有不同的代码效果

    android {
        defaultConfig {
        buildConfigField "boolean", "AUTO_UPDATES", "true"
    }

    productFlavors {
        wandoujia {
            buildConfigField "boolean", "AUTO_UPDATES", "false"
        }        
    }
    
###使用不同的包名

    productFlavors {
        hd {
            applicationId "com.meituan.group.hd"
        }
    }

###使用不同的应用名

    android {
        productFlavors {
            wandoujia { 
            }
        }
    }

上面的配置会默认src/wandoujia目录为wandoujia flavor的dataSet。

接下来，在src目录内创建wandoujia目录，并添加如下应用名字符串资源（src/wandoujia/res/values/appname.xml）：

    <resources>
        <string name="app_name">美团团购</string>
    </resources>
    
默认的应用名字符串资源如下（src/main/res/values/strings.xml）:

    <resources>
        <string name="app_name">美团</string>
    </resources>
最后，运行gradle assembleWandoujia命令即可生成应用名为美团团购的应用了。    
    
###使用第三方SDK

    android {
        productFlavors {
            qihu360 {
            }
        }
    }
    ...
    dependencies {
        provided 'com.qihoo360.union.sdk:union:1.0'
        qihu360Compile 'com.qihoo360.union.sdk:union:1.0'
    }
上例添加了名为qihu360的flavor，并且指定编译和运行时都依赖com.qihoo360.union.sdk:union:1.0。而其他渠道只是在构建的时候依赖该SDK，打包的时候并不会添加它。

接下来，需要在代码中使用反射技术判断应用程序是否添加了该SDK，从而决定是否要显示360 SDK提供的精品应用。部分代码如下：

    class MyActivity extends Activity {
        private boolean useQihuSdk;
    
        @override
        public void onCreate(Bundle savedInstanceState) {
            try {
                Class.forName("com.qihoo360.union.sdk.UnionManager");
                useQihuSdk = true;
            } catch (ClassNotFoundException ignored) {
    
            }
        }
    }
    
最后，运行gradle assembleQihu360命令即可生成包含360精品应用模块的渠道包了。    
    
##问题：
使用check findBugs checkStyle pmd插件
美团打包方式，是否可以修改包名，修改应用名，修改mainfest.xml中第三方参数   
build.gradle 拆分（暂时没）   
gradle 能做什么插件？    

##参考：
[http://tech.meituan.com/mt-apk-adaptation.html](http://tech.meituan.com/mt-apk-adaptation.html)   
[http://tools.android.com/tech-docs/new-build-system/user-guide](http://tools.android.com/tech-docs/new-build-system/user-guide)

 