Cloud Plugins for Play! Framework [![Build Status](https://travis-ci.org/lukaszbudnik/play-cloud-plugins.png?branch=master)](https://travis-ci.org/lukaszbudnik/play-cloud-plugins)
==================

Cloud Plugins project delivers 3 (for now) cloud plugins:

* Cloudinary - cloud-based image management and manipulation
* Mailgun - cloud-based mail services over HTTPS
* Twilio - telecommunication cloud - currently supports sending SMS messages

## Installation (forked repo to add it into github pages so it can be released)

Start by adding the plugin as a dependency, in your `project/Build.scala`

    val appDependencies = Seq(
      "play-cloud-plugins" % "play-cloud-plugins_2.9.1" % "1.0-SNAPSHOT"
    )

Then add the repository into your play project resolvers (it is hosted on github taking advantage of github pages)

    val main = PlayProject(appName, appVersion, appDependencies, mainLang = SCALA).settings(
      ....
      resolvers += Resolver.url("FireMuzzy GitHub Play Repository", url("http://firemuzzy.github.com/releases/"))(Resolver.ivyStylePatterns),
      ....
    )

## Configuration
### Environment variables

Configuration is pretty straight forward. All you have to do is set proper environment variables:

    export CLOUDINARY_URL="cloudinary://AAA:BBB@CCC"
    export MAILGUN_API_KEY="key-ABC"
    export MAILGUN_SMTP_LOGIN="postmaster@ABC.mailgun.org"
    export TWILIO_APPLICATION_SID=SID
    export TWILIO_AUTH_TOKEN=TOKEN

If you use Heroku, Cloudinary and Mailgun environment variables are provided out of the box after you install proper add-ons. Twilio is not (yet) supported by Heroku so you have to create these environment variables yourself.

### play.plugins

Next you have to configure Play! plugins. Create _conf/play.plugins_ file and paste:

    20000:plugins.cloudimage.CloudinaryCloudImagePlugin
    20001:plugins.emailnotifier.MailgunEmailNotifierPlugin
    20002:plugins.smsnotifier.TwilioSmsNotifierPlugin

That's all.

## Usage

Each Plugin has a service property which you should use. Please see specs for full source code examples.

### Cloudinary

    val cloudImageService = use[CloudImagePlugin].cloudImageService
    val uploadResponse = cloudImageService.upload(fileName, contents)
    val publicId = uploadResponse.asInstanceOf[CloudImageSuccessResponse].publicId
    val destroyResponse = cloudImageService.destroy(publicId)
    val transformationUrl = cloudImageService.getTransformationUrl(url, transformationProperties)

### Mailgun

    val emailNotifierService = use[EmailNotifierPlugin].emailNotifierService
    val response = emailNotifierService.send(from, to, subject, textMessage, htmlMessage)

### Twilio

    val smsNotifierService = use[SmsNotifierPlugin].smsNotifierService
    val response = smsNotifierService.send(from, to, text)

## Behavioural Tests

You can run Spec2 tests by executing _play test_ command. Behavioural tests also show you how to use these plugins. In addition play-cloud-plugins project uses Travis CI. Current status is  [![Build Status](https://secure.travis-ci.org/lukasz-budnik/play-cloud-plugins.png?branch=master)](http://travis-ci.org/lukasz-budnik/play-cloud-plugins)
