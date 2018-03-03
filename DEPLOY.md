# Deploy notes

## packaging
Images must stay less than about 200K. You should aggressively exclude
anything that is not absolutely necessary. Only directories and explicit
full file paths can be excluded.

## domain-manager

Unfortunately, there are a number of bugs with domain-manager.

1) It will look for the first certificate for a given domain match,
ths may result in it pulling an expired (or soon to be expired) cert from
the certificate manager. Apparently, you can't specify an ARN for a valid
certificate.
2) The cert MUST be in the region that you're deploying to.


## Post deploy steps

* Go to the [Amazon API Gateway:Custom Domain Names](https://console.aws.amazon.com/apigateway/home?region=us-east-1#/custom-domain-names) section
* Create a custom domain for `pushbox.dev.mozaws.net`
* select the appropriate ACM certificate for "*.dev.mozaws.net" (You may
want to [verify the cert identifier from ACM](https://console.aws.amazon.com/acm/home))
* Set the **Base Path Mappings**
    * Leave **Path** empty
    * Set **Destination** to `dev-pushbox...`.
    * Set the Stage to `dev`

* Click ***Save***
* Copy the **Target Domain Name** to your clipboard.

* Go to the [Route 53 panel](https://console.aws.amazon.com/route53/home#resource-record-sets:Z3GEB01DYXZM0A)
* Click ***Create Record Set***
    * **Name:** pushbox.dev.mozaws.net
    * **Type:** A - IPv4 address
    * **Alias:** Yes
    * **Target:** Paste the *Target Domain Name* from above.
* Click ***Create***

It may take 40 minutes before the domain is available.
