# React Native APK Installer

Install an android APK from your react-native project.
This project is based on `react-native-android-library-boilerplate` and `react-native-install-apk`

# Installing ApkInstaller

1. Add `react-native-apk-installer` to your project

  - Install directly from npm: `npm install --save react-native-apk-installer`.
  - Or do `npm install --save git+https://github.com/null--/react-native-apk-installer.git` in your main project.

2. Link the library:

  - Add the following to `android/settings.gradle`:

    ```
      include ':react-native-apk-installer'
      project(':react-native-apk-installer').projectDir = new File(settingsDir, '../node_modules/react-native-apk-installer/android')
    ```

  - Add the following to `android/app/build.gradle`:

    ```xml
      ...

      dependencies {
          ...
          compile project(':react-native-apk-installer')
      }
    ```

  - Add the following to `android/app/src/main/java/**/MainApplication.java`:

    ```java
      ...
      import com.cnull.apkinstaller.ApkInstallerPackage;  // add this for react-native-apk-installer

      public class MainApplication extends Application implements ReactApplication {

          @Override
          protected List<ReactPackage> getPackages() {
              return Arrays.<ReactPackage>asList(
                  new MainReactPackage(),
                  ...
                  new ApkInstallerPackage()     // add this for react-native-apk-installer
              );
          }
      }
    ```

4. Simply `import ApkInstaller from 'react-native-apk-installer'` (You might also need to install `react-native-fs` package).

  ```javascript
    import RNFS from 'react-native-fs';
    import ApkInstaller from 'react-native-apk-installer'

     try {
         var filePath = RNFS.CachesDirectoryPath + '/com.example.app.apk';
         var download = RNFS.downloadFile({
           fromUrl: 'http://example.com/com.example.app.apk',
           toFile: filePath,
           progress: res => {
               console.log((res.bytesWritten / res.contentLength).toFixed(2));
           },
           progressDivider: 1
         });

         download.promise.then(result => {
           if(result.statusCode == 200) {
             console.log(filePath);
             ApkInstaller.install(filePath);
           }
         });
     }
     catch(error) {
         console.warn(error);
     }
  ```
