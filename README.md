
# react-native-translate

Integrates [I18n.js](https://github.com/fnando/i18n-js) with React Native. Uses the user preferred locale as default.
<br/>
<br/>

## Installation

**Using yarn (recommended)**

`$ yarn add react-native-translate`

**Using npm**

`$ npm install react-native-translate --save`

## Automatic setup

After installing the npm package you need to link the native modules.

If you're using React-Native >= 0.29 just link the library with the command `react-native link react-native-translate`.

If you're using React-Native < 0.29, install [rnpm](https://github.com/rnpm/rnpm) with the command `npm install -g rnpm` and then link the library with the command `rnpm link`.

If you're having any issue you can also try to install the library manually as follows.

## Automatic setup with Cocoapods

After installing the npm package, add the following line to your Podfile

```ruby
pod 'RNI18n', :path => '../node_modules/react-native-translate'
```

and run

```
pod install
```

## Manual setup

### iOS

Add `RNI18n.xcodeproj` to **Libraries** and add `libRNI18n.a` to **Link Binary With Libraries** under **Build Phases**.  
[More info and screenshots about how to do this is available in the React Native documentation](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#content).

You also need to add the **localizations** you intend to support to your iOS project. To do that open your Xcode project:

```
$ open <your-project>.xcodeproj
```

And add the localizations you will support as shown here:

![adding locales](https://github.com/AlexanderZaytsev/react-native-translate/blob/master/docs/adding-locales.png?raw=true)

### Android

Add `react-native-translate` to your `./android/settings.gradle` file as follows:

```gradle
include ':app', ':react-native-translate'
project(':react-native-translate').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-translate/android')
```

Include it as dependency in `./android/app/build.gradle` file:

```gradle
dependencies {
    // ...
    compile project(':react-native-translate')
}
```

Finally, you need to add the package to your MainApplication (`./android/app/src/main/java/your/bundle/MainApplication.java`):

```java
import com.AlexanderZaytsev.RNI18n.RNI18nPackage; // <-- Add to ReactNativeI18n to the imports

// ...

@Override
protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        // ...
        new RNI18nPackage(), // <-- Add it to the packages list
    );
}

// ...
```

After that, you will need to recompile your project with `react-native run-android`.


## Usage

```javascript
import I18n from 'react-native-translate';
// OR const I18n = require('react-native-translate').default

class Demo extends React.Component {
  render() {
    return <Text>{I18n.t('greeting')}</Text>;
  }
}

// Enable fallbacks if you want `en-US` and `en-GB` to fallback to `en`
I18n.fallbacks = true;

I18n.translations = {
  en: {
    greeting: 'Hello!',
  },
  de: {
    greeting: 'Hallo!',
  },
};
```

This will render `Hello!` for devices with the English locale, and `Hallo!` for devices with the German locale.

## Usage with multiple location files

```javascript
// app/i18n/locales/en.js

export default {  
  greeting: 'Hallo!'
};

// app/i18n/locales/de.js

export default {  
  greeting: 'Hallo!'
};

// app/i18n/i18n.js

import I18n from 'react-native-translate';
import en from './locales/en';
import de from './locales/de';

I18n.fallbacks = true;

I18n.translations = {
  en,
  de
};

export default I18n;

// usage in component

import I18n from 'app/i18n/i18n';

class Demo extends React.Component {
  render () {
    return (
      <Text>{I18n.t('greeting')}</Text>
    )
  }
}
```

### Fallbacks

When fallbacks are enabled (which is generally recommended), `i18n.js` will try to look up translations in the following order (for a device with `de_DE` locale):

- en-US
- en
- de

**Note**: iOS 8 locales use underscored (`de_DE`) but `i18n.js` locales are dasherized (`de-DE`). This conversion is done automatically for you.

```javascript
I18n.fallbacks = true;

I18n.translations = {
  en: {
    greeting: 'Hello!',
  },
  'en-GB': {
    greeting: 'Hello, How are you',
  },
};
```

For a device with a `en_GB` locale this will return `Hello, How are you'`, for a device with a `en_US` locale it will return `Hello!`.

### Device's locales

You can get the user preferred locales with the `getLanguages` method:

```javascript
import { getLanguages } from 'react-native-translate';

getLanguages().then(languages => {
  console.log(languages); // ['en-US', 'en']
});
```

### I18n.js documentation

For more info about I18n.js methods (`localize`, `pluralize`, etc) and settings see [its documentation](https://github.com/fnando/i18n-js#setting-up).

## Licence

MIT
