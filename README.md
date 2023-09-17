
# Инструкция по сборке Cordova для плеймаркета или RuStore

Добавить в конфиг Cordova config.xml (можно и не добавлять)
```
<preference name="AndroidLaunchMode" value="singleTask" />
```

сборка apk (для RuStore)
```
cordova build android --release -- --packageType=apk
```

сборка bandle (для PlayMarket)
```
cordova build android --release -- --packageType=bandle
```

Создать ключ чтобы подписать конкретное приложение
```
keytool -genkeypair -v -keystore myApp.jks -alias myApp -keyalg RSA -keysize 2048 -validity 10000
```

Подписать .aab (бандл)
```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore myApp.jks platforms/android/app/build/outputs/bundle/release/app-release.aab myApp
```

Подписать .apk
```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore myApp.jks platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk myApp

```

Можно сделать ключ 256
```
jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore myApp.jks platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk myApp

```

Оптимизировать apk (app-release-signed.apk можно будет заливать куда нужно)
```
zipalign -v 4 platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk platforms/android/app/build/outputs/apk/release/app-release-signed.apk
```

Для PlayMarket

Качнуть с google открытый ключ encryption_public_key.pem (в консоли будет ссылка) и сделать my-app.zip который потом загрузить в google.console
```
java -jar pepk.jar --keystore=myApp.jks --alias=mmyApp --output=myApp.zip --include-cert --rsa-aes-encryption --encryption-key-path=encryption_public_key.pem
```

После того как google.console примет ключ, можно заливать подписанный файл app-release.aab на проверку

Примечание:
Файл encryption_public_key.pem вы можете скачать в разделе **Разрешить Google Play управлять вашим ключом подписи приложения** в google.console
Там же дана ссылка на файл pepk.jar но этот файл с ошибкой, в этом примере правильный файл
