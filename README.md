
# cordova-playmarket-instruction

собрать релиз apk
```
cordova build android --release -- --packageType=apk
```

собрать релиз bandle
```
cordova build android --release -- --packageType=bandle
```

Создать ключ чтобы подписать конкретное приложение
```
keytool -genkeypair -v -keystore my-app.jks -alias my-app-alias -keyalg RSA -keysize 2048 -validity 10000
```

Подписать бандл
```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-app.jks platforms/android/app/build/outputs/bundle/release/app-release.aab my-app-alias
```

Подписать apk (при необходимости)
```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -my-app.jks platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk my-app-alias
```

Оптимизировать apk (app-release-signed.apk можно будет заливать куда нужно)
```
zipalign -v 4 platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk platforms/android/app/build/outputs/apk/release/app-release-signed.apk
```


Качнуть с google открытый ключ encryption_public_key.pem (в консоли будет ссылка) и сделать my-app.zip который потом загрузить в google.console
```
java -jar pepk.jar --keystore=my-app.jks --alias=my-app-alias --output=my-app.zip --include-cert --rsa-aes-encryption --encryption-key-path=encryption_public_key.pem
```

После того как google.console примет ключ, можно заливать подписанный файл app-release.aab на проверку
