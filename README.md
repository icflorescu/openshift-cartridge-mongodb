# Custom MongoDB cartridge for OpenShift

![mongodb-openshift](https://cloud.githubusercontent.com/assets/581999/13374624/f9509bc2-dd92-11e5-8068-a87c9c3f6312.png)

This is a custom OpenShift cartridge providing MongoDB 3.2.x + WiredTiger storage engine.

## Why

Because the standard OpenShift MongoDB cartridge is stuck at 2.4.

## When to use

When you need a quick and unsofisticated solution to run your application with the latest MongoDB version.

## How to

### Using [web console](https://openshift.redhat.com/app/console/applications)

Open your application in the [web console](https://openshift.redhat.com/app/console/applications), go to **"See the list of cartridges you can add"**, paste the URL below in **"Install your own cartridge"** textbox at the bottom of the page and click "Next".

    https://raw.githubusercontent.com/icflorescu/openshift-cartridge-mongodb/master/metadata/manifest.yml

Then you can use `MONGODB_URL` environment variable to connect from an application running in the main web cartridge.

To keep things flexible, the connection string stored in `MONGODB_URL` ends with `/` and **does not include a database name**. Use your application logic to name/create your database(s) as needed.

For instance, here's how you'd do it in a Node.js application using [Mongoose ODM](http://mongoosejs.com/):

    var mongoose = require('mongoose');
    ...
    // Mongoose will create database_name if necessary
    mongoose.connect(process.env.MONGODB_URL + 'database_name', { db: { nativeParser: true } });

### Using the command line

Make sure you have `rhc` installed (see [here](https://developers.openshift.com/en/managing-client-tools.html)).

If you want to **add** a MongoDB instance based on this cartridge to an existing application:

    rhc cartridge-add https://raw.githubusercontent.com/icflorescu/openshift-cartridge-mongodb/master/metadata/manifest.yml \
      --app appname

...where `appname` is the name of your application.

To **create** an app based on the standard Node.js v0.10 cartridge and this one, run:

    rhc app create appname \
      nodejs-0.10 \
      https://raw.githubusercontent.com/icflorescu/openshift-cartridge-mongodb/master/metadata/manifest.yml

If you want to use **the latest Node.js version**, you'll have to use a [custom Node.js cartridge](https://github.com/icflorescu/openshift-cartridge-nodejs) as well:

    rhc app create appname \
      https://raw.githubusercontent.com/icflorescu/openshift-cartridge-nodejs/master/metadata/manifest.yml \
      https://raw.githubusercontent.com/icflorescu/openshift-cartridge-mongodb/master/metadata/manifest.yml

## Notes

- Can't guarantee this cartridge is production-ready. Some people use it though (on **their own responsibility**).
- This is a lean cartridge. To save space, just `mongod` is installed. No client libraries, no `mongo` console. **If you need external access to your data, use `rhc port-forward`**.
- By default, the underlying MongoDB instance will accept unauthenticated access, which should be fine for most typical usage scenarios. See [the discussion here](https://github.com/icflorescu/openshift-cartridge-mongodb/issues/1) for more info.
- Can't think of a safe way to make this cartridge auto-updatable. For now we'll just have to use `mongodump`, destroy the cartridge, install the new version, then do a `mongorestore`.
- Don't hesitate to make a pull-request with an updated version in [this file](https://github.com/icflorescu/openshift-cartridge-mongodb/blob/master/metadata/manifest.yml#L4) if you notice this cartridge version is behind the latest stable [official MongoDB linux binary](http://www.mongodb.org/downloads).

## FAQ

**Q**: I'm getting the error *Cannot download, must be smaller than 20480 bytes* while trying to deploy the cartridge to OpenShift. What am I doing wrong?

**A**: You're probably trying to use the URL `https://github.com/icflorescu/openshift-cartridge-mongodb` instead of
`https://raw.githubusercontent.com/icflorescu/openshift-cartridge-mongodb/master/metadata/manifest.yml`. A common mistake for people not paying sufficient attention while trying to use a custom cartridge for the first time.

---

**Q**: How do I change the default `mongod` options?

**A**: `ssh` to the MongoDB gear, edit the [control script](https://github.com/icflorescu/openshift-cartridge-mongodb/blob/master/bin/control) with `nano mongodb/bin/control` and make sure to restart afterwards.

---

**Q:** How can I `ssh` to a MongoDB cartridge running in a secondary application gear?

**A:** You can discover the secondary gear URL by running `rhc app show appname --gears`.

## Related

Since you're here, chances are you might also be interested in this [custom Node.js cartridge](https://github.com/icflorescu/openshift-cartridge-nodejs).

## Credits

See contributors [here](https://github.com/icflorescu/openshift-cartridge-nodejs/graphs/contributors).

If you find this repo useful, don't hesitate to give it a star and [spread the word](http://twitter.com/share?text=Checkout%20this%20custom%20MongoDB%20cartridge%20for%20OpenShift!&amp;url=http%3A%2F%2Fgithub.com/icflorescu/openshift-cartridge-mongodb&amp;hashtags=mongodb,openshift,nodejs&amp;via=icflorescu).

## License

The [ISC License](http://github.com/icflorescu/openshift-cartridge-mongodb/blob/master/LICENSE).
