retranslate
===========
[![CircleCI](https://circleci.com/gh/Tankenstein/retranslate/tree/master.svg?style=shield)](https://circleci.com/gh/Tankenstein/retranslate/tree/master) [![Coverage Status](https://coveralls.io/repos/github/Tankenstein/retranslate/badge.svg)](https://coveralls.io/github/Tankenstein/retranslate)

Real simple translations for react. Lightweight (< 10 KB gzipped) and with no dependencies.

## Usage

First, install retranslate using
+ npm: `npm install --save retranslate`
+ or yarn: `yarn add retranslate`

```javascript
import { Message, Provider as TranslationProvider, withTranslations } from 'retranslate';

import SomeComponent from './someComponent';
import { getLanguage } from './config'; // some functionality to select a language, in this example, hopefully returning en or et.
import { version } from '../../package.json';

// You can import these from a json file that tools like crowdin can use, like
// import enTranslations from './translations.en.json';
// import etTranslations from './translations.et.json';
const translations = {
  en: {
    'main.heading': 'retranslate #{{ versionNumber }}',
    'main.subtitle': 'Real simple translations for react.',
    'current.language': 'Your current language is {{ currentLanguage }}',
  },
  et: {
    'main.heading': 'retranslate #{{ versionNumber }}',
    'main.subtitle': 'Väga lihtsad tõlked reactile.',
    'current.language': 'Teie hetke keel on {{ language }}',
  },
};

// You can use the withTranslations higher-order component to get a hold of the current language and the translate function.
const LanguageShower = withTranslations(({ translations: { translate, language } }) => (
  <p>{translate('current.language', { currentLanguage: language })}</p>
));

const App = () => (
  <TranslationProvider messages={translations} language={getLanguage()} fallbackLanguage="en">
    <h1><Message params={{ versionNumber: version }}>main.heading</Message></h1>
    <SomeComponent /> // this can also use the Message component, since there is a Provider up the tree.
    <Message className="lead">main.subtitle</Message>
    <LanguageShower />
  </TranslationProvider>
);
```

### Provider

retranslate is configured using the `Provider`. You pass `Provider` `messages`, a `language` and a `fallbackLanguage` (just in case). Wrap your application with `Provider` to make retranslate work.

### Message

retranslate uses `Message` to actually translate your messages. It uses the children you give it as the key to use to get translations.

### withTranslations

`withTranslations` is a higher order component that you can use to access translation functionality and language manually.

## Potential questions

+ Async loading

  I don't want to build this into retranslate. I would keep handling this on the application side.

+ Logic in templates, such as `{{ thing + 1 }}`

  I would not like to do this, as it makes the whole application harder to test and maintain.

+ Plurals

  This is something that will be worked on.

+ Compiled templates for even better performance

  Also something that will probably be worked on.

## Contributing

Use the `test` and `build` script to test the library. Make a pull request, and it will be automatically checked by CircleCI, Coveralls, and @Tankenstein. When you make a production code change, make sure to increment the version in `package.json` according to semver. As your branch is merged, a release will automatically be made.

retranslate is licensed under MIT.
