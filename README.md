# Visual Recognition demo
[![Build Status](https://travis-ci.org/watson-developer-cloud/visual-recognition-nodejs.svg?branch=master)](https://travis-ci.org/watson-developer-cloud/visual-recognition-nodejs?branch=master)
[![codecov.io](https://codecov.io/github/watson-developer-cloud/visual-recognition-nodejs/coverage.svg?branch=master)](https://codecov.io/github/watson-developer-cloud/visual-recognition-nodejs?branch=master)

The [Visual Recognition][visual_recognition_service] service uses deep learning algorithms to analyze images for scenes, objects, faces, text, and other subjects that can give you insights into your visual content. You can organize image libraries, understand an individual image, and create custom classifiers for specific results that are tailored to your needs.

Give it a try! Click this button to fork into IBM DevOps Services and deploy your own copy of this application on Bluemix.

[![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/watson-developer-cloud/visual-recognition-nodejs)

## Getting started

1.  Create a Bluemix account:

    [Sign up][sign_up] in Bluemix or use an existing account. If you are creating an account, you'll have to create an organization and a space. You'll be prompted for these when you first log in.

1.  Download and install the [Cloud-foundry CLI][cloud_foundry] tool.
1.  In the `manifest.yml` file, change the name `visual-recognition-demo` to something unique.

    ```yml
    ---
    declared-services:
      visual-recognition-service:
        label: watson_vision_combined
        plan: free
    applications:
    - name: <application-name>
      path: .
      command: npm start
      memory: 512M
      services:
      - visual-recognition-service
      env:
    NODE_ENV: production
    ```

    The name determines your initial application URL. For example `<application-name>.mybluemix.net`.

1.  Connect to Bluemix in the command line tool.

    ```sh
    $ cf login -u <your user ID>
    $ cf api https://api.ng.bluemix.net
    ```

1.  Create the Visual Recognition service in Bluemix.

    ```sh
    $ cf create-service watson_vision_combined free visual-recognition-service
    ```

1.  Push it live.

    ```sh
    $ cf push
    ```

1.  Go to `<application-name>.mybluemix.net` to check out the demo.

See the [API reference](http://www.ibm.com/watson/developercloud/visual-recognition/api/v3/) documentation for more details, including code snippets and references.

## Running locally

The application uses [Node.js][node_js] and [npm][npm], so download and install them as part of the following steps.

1.  Create a file named .env in the root directory of the project with the following content:

    ```none
    VISUAL_RECOGNITION_API_KEY=<api_key>
    ```

    You can see the `<api_key>` of your application using the `cf env` command:

    ```sh
    $ cf env <application-name>
    ```
    Example output:
    ```sh
    System-Provided:
    {
    "VCAP_SERVICES": {
      "watson_vision_combined": [{
          "credentials": {
            "url": "<url>",
            "api_key": "<api_key>",
          },
        "label": "visual_recognition",
        "name": "visual-recognition-service",
        "plan": "standard"
     }]
    }
    }
    ```

1.  Install [Node.js][node_js]. Installing Node JS also installs [npm][npm].

1.  Go to the project folder in a terminal and run this command:

    ```sh
    $ npm install
    ```

1.  Start the application:

    ```sh
    $ npm start
    ```

1.  Go to `http://localhost:3000` to view the demo.

## Troubleshooting

To view your logs and troubleshoot your Bluemix application, run this command:

```sh
$ cf logs <application-name> --recent
```

## Environment variables

- `VISUAL_RECOGNITION_API_KEY`: This is the API key for the Visual Recognition service. It is used if you don't have a key in your bluemix account.
- `PRESERVE_CLASSIFIERS`: Set this variable if you don't want classifiers to be deleted after one hour *(optional)*.
- `GOOGLE_ANALYTICS`: Set this variable to your Google Analytics key if you want analytics enabled.  *(optional)*.
- `PORT`: The port that the server will run on *(optional, defaults to `3000`)*.
- `OVERRIDE_CLASSIFIER_ID`: Set this variable to a classifer ID if you always want to use a custom classifier. This classifier is used instead of training a new one *(optional)*.

## Changing the included images

### Sample images

The sample images are the first 7 images when the site loads.  They are called from a Jade mixin found in `views/mixins/sampleImages.jade`.  You can replace the images in `public/images/samples`. The images are numbered 1 - 7 and are formatted as `.jpg` files.

### Custom classifier bundles

Adding new or different custom classifer bundles is more involved. You can follow the template of the existing bundles found in `views/includes/train.jade`.

Or, you can train a custom classifier using either the API or the form, and then use the classifier ID.

## Getting the classifier ID

When you train a custom classifier, the name of the classifier is displayed in the test form.

![Classifier ID tooltip](screengrab-tooltip.png)

When you hover your mouse over the classifier name, the classifier ID is displayed in the tooltip. You can also click the name and it toggles between classifier name and classifier ID.

You can then use this custom classifier ID by placing it after the hash in the request URL.

For example, let's say you are running the system locally, so the base URL is `http://localhost:3000`. You then train a classifier. This new classifier has the ID `SatelliteImagery_859438478`. To use this classifier instead of training a new one, navigate to `http://localhost:3000/train#SatelliteImagery_859438478` and use the training form with your existing classifier.

## License

This sample code is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md).

## Open Source @ IBM

Find more open source projects on the [IBM Github Page](http://ibm.github.io/).

### Privacy notice

This node sample web application includes code to track deployments to Bluemix and other Cloud Foundry platforms. The following information is sent to a [Deployment Tracker][deploy_track_url] service on each deployment:

- Application Name (`application_name`)
- Space ID (`space_id`)
- Application Version (`application_version`)
- Application URIs (`application_uris`)

This data is collected from the `VCAP_APPLICATION` environment variable in IBM Bluemix and other Cloud Foundry platforms. This data is used by IBM to track metrics around deployments of sample applications to IBM Bluemix. Only deployments of sample applications that include code to ping the Deployment Tracker service will be tracked.

### Disabling deployment tracking

Deployment tracking can be disabled by removing `require('cf-deployment-tracker-client').track();` from the beginning of the `server.js` file at the root of this repo.

[deploy_track_url]: https://github.com/cloudant-labs/deployment-tracker
[cloud_foundry]: https://github.com/cloudfoundry/cli
[visual_recognition_service]: https://www.ibm.com/watson/services/visual-recognition/
[sign_up]: https://console.ng.bluemix.net/registration/
[getting_started]: https://console.bluemix.net/docs/services/watson/index.html
[node_js]: http://nodejs.org/
[npm]: https://www.npmjs.com
