# 动态配置gradle 打包参数
## 动态添加参数
    buildConfigField "String", "API_URL", "\"http://example.com/api\""
    buildConfigField "boolean", "LOG_HTTP_CALLS", "false"
## 修改打包apk输出名称
    applicationVariants.all { variant ->
                variant.outputs.all { output ->

                    def outputFile = output.outputFile
                    if (outputFile != null && outputFile.name.endsWith('.apk')) {
                        def fileName = "${variant.productFlavors[0].name}-V${defaultConfig.versionName}.apk"
//                        output.outputFile = new File(outputFile.parent, fileName)//3.0之前 3.0直接赋值outputFileName名称即可
                        outputFileName = fileName
                    }
                }
            }
## 多渠道打包
    3.0添加参数flavorDimensions 在defaultConfig中随便配置一个名称即可，也可配置多个名称
    例：flavorDimensions "release"
    productFlavors {
            client {
                dimension "release"
                manifestPlaceholders = [app_icon: "@mipmap/ic_launcher_round", app_name: "@string/client_app_name"]

            }
            company {
                dimension "release"
                manifestPlaceholders = [app_icon: "@mipmap/company_ic_launcher", app_name: "@string/company_app_name"]
            }
        }

## 动态修改打包名称和图标
    例：需要先定义占位名称
    <application
            android:allowBackup="true"
            android:icon="${app_icon}"
            android:label="${app_name}"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme">
            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>

    在gradle中动态配置占位符代表的东西
    manifestPlaceholders = [app_icon: "@mipmap/ic_launcher_round", app_name: "@string/client_app_name"]

## 也可以添加方法
    例：获取打包时间
    def releaseTime() {
        return new Date().format("yyyyMMdd", TimeZone.getTimeZone("UTC"))
    }