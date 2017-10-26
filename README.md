<img src="https://avatars2.githubusercontent.com/u/2810941?v=3&s=96" alt="Google Cloud Platform logo" title="Google Cloud Platform" align="right" height="96" width="96"/>

# Google Prediction API: Node.js Client

[![release level](https://img.shields.io/badge/release%20level-deprecated-red.svg?style&#x3D;flat)](https://cloud.google.com/terms/launch-stages)
[![CircleCI](https://img.shields.io/circleci/project/github/googleapis/nodejs-prediction.svg?style=flat)](https://circleci.com/gh/googleapis/nodejs-prediction)
[![AppVeyor](https://ci.appveyor.com/api/projects/status/github/googleapis/nodejs-prediction?branch=master&svg=true)](https://ci.appveyor.com/project/googleapis/nodejs-prediction)
[![codecov](https://img.shields.io/codecov/c/github/googleapis/nodejs-prediction/master.svg?style=flat)](https://codecov.io/gh/googleapis/nodejs-prediction)

> Node.js idiomatic client for [Prediction API][product-docs].

The [Cloud Prediction API](https://cloud.google.com/prediction/docs) provides a RESTful API to build Machine Learning models.

| :warning: Deprecated Module |
| --- |
| This library is **deprecated**. The API will be shut down on April 30, 2018. See the [Prediction API End of Life FAQ](https://cloud.google.com/prediction/docs/end-of-life-faq) for more information. |

* [Prediction API Node.js Client API Reference][client-docs]
* [Prediction API Documentation][product-docs]

Read more about the client libraries for Cloud APIs, including the older
Google APIs Client Libraries, in [Client Libraries Explained][explained].

[explained]: https://cloud.google.com/apis/docs/client-libraries-explained

**Table of contents:**

* [Quickstart](#quickstart)
  * [Before you begin](#before-you-begin)
  * [Installing the client library](#installing-the-client-library)
  * [Using the client library](#using-the-client-library)
* [Samples](#samples)
* [Versioning](#versioning)
* [Contributing](#contributing)
* [License](#license)

## Quickstart

### Before you begin

1.  Select or create a Cloud Platform project.

    [Go to the projects page][projects]

1.  Enable billing for your project.

    [Enable billing][billing]

1.  Enable the Google Prediction API API.

    [Enable the API][enable_api]

1.  [Set up authentication with a service account][auth] so you can access the
    API from your local workstation.

[projects]: https://console.cloud.google.com/project
[billing]: https://support.google.com/cloud/answer/6293499#enable-billing
[enable_api]: https://console.cloud.google.com/flows/enableapi?apiid=prediction.googleapis.com
[auth]: https://cloud.google.com/docs/authentication/getting-started

### Installing the client library

    npm install --save @google-cloud/prediction

### Using the client library

```javascript
var google = require('googleapis');

function auth(callback) {
  google.auth.getApplicationDefault(function(err, authClient) {
    if (err) {
      return callback(err);
    }

    // The createScopedRequired method returns true when running on GAE or a
    // local developer machine. In that case, the desired scopes must be passed
    // in manually. When the code is  running in GCE or GAE Flexible, the scopes
    // are pulled from the GCE metadata server.
    // See https://cloud.google.com/compute/docs/authentication for more
    // information.
    if (authClient.createScopedRequired && authClient.createScopedRequired()) {
      // Scopes can be specified either as an array or as a single,
      // space-delimited string.
      authClient = authClient.createScoped([
        'https://www.googleapis.com/auth/prediction',
      ]);
    }
    callback(null, authClient);
  });
}

/**
 * @param {string} phrase The phrase for which to predict sentiment,
 * e.g. "good morning".
 * @param {Function} callback Callback function.
 */
function predict(phrase, callback) {
  auth(function(err, authClient) {
    if (err) {
      return callback(err);
    }
    var hostedmodels = google.prediction({
      version: 'v1.6',
      auth: authClient,
    }).hostedmodels;
    // Predict the sentiment for the provided phrase
    hostedmodels.predict(
      {
        // Project id used for this sample
        project: '414649711441',
        hostedModelName: 'sample.sentiment',
        resource: {
          input: {
            // Predict sentiment of the provided phrase
            csvInstance: phrase.split(/\s/gi),
          },
        },
      },
      function(err, prediction) {
        if (err) {
          return callback(err);
        }

        // Received prediction result
        console.log(`Sentiment for "${phrase}": ${prediction.outputLabel}`);
        callback(null, prediction);
      }
    );
  });
}
```

## Samples

Samples are in the [`samples/`](https://github.com/googleapis/nodejs-prediction/blob/master/samples) directory. The samples' `README.md`
has instructions for running the samples.

| Sample                      | Source Code                       |
| --------------------------- | --------------------------------- |
| Hosted Models | [source code](https://github.com/googleapis/nodejs-prediction/blob/master/samples/hostedmodels.js) |

The [Prediction API Node.js Client API Reference][client-docs] documentation
also contains samples.

## Versioning

This library follows [Semantic Versioning](http://semver.org/).

This library is **deprecated**. This means that it is no longer being
actively maintained and the only updates the library will receive will
be for critical security issues. 

More Information: [Google Cloud Platform Launch Stages][launch_stages]

[launch_stages]: https://cloud.google.com/terms/launch-stages

## Contributing

Contributions welcome! See the [Contributing Guide](.github/CONTRIBUTING.md).

## License

Apache Version 2.0

See [LICENSE](LICENSE)

[client-docs]: https://cloud.google.com/nodejs/docs/reference/prediction/latest/
[product-docs]: https://cloud.google.com/prediction/docs
