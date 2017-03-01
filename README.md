# federalist-redirects

Redirects traffic from custom domains to Federalist. Note that despite the name of this app, it is not a core component of the Federalist system.

## Usage

To deploy the app:

    $ cf push -f manifest.yml

## Adding a new redirect

There are four steps involved in adding a new redirect to the app. This app is designed to be used in conjunction with CloudFront distributions created using cloud.gov's [CDN service](https://cloud.gov/docs/services/cdn-route/). However, since the app relies on nginx' virtual host support, we require steps 3 and 4 in addition to just updating the app (step 1) and adding a new CloudFront distribution (step 2).

1. Create a new `server` stanza to [nginx.conf](https://github.com/18F/federalist-redirects/blob/master/nginx.conf) and redeploy the app.
2. Create a new instance of the [CDN service](https://cloud.gov/docs/services/cdn-route/).
3. It is necessary to ensure the `Host` header is forwarded to the app. At present, this requires logging into the AWS console and editing the CloudFront distribution manually. Once you've selected the correct distribution in the AWS console, select the `Behaviors` tab, edit the `Default (*)` behavior, choose the `Whitelist` option in the `Forward Headers` section, and add the `Host` header to the list of whitelisted headers. Save your change.
4. Finally, ensure the domain is mapped properly to the redirects app, as described in the [Custom domains - manual methods](https://cloud.gov/docs/apps/custom-domains/#manual-method) section of the cloud.gov documentation.
