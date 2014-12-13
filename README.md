# Custom MongoDB application cartridge for OpenShift

This is a custom MongoDB cartridge.

## Why

Because the standard OpenShift MongoDB cartridge is stuck at 2.4.

## When to use

When you need a quick and unsofisticated solution to run your application with the latest MongoDB version.

## How to

To install this cartridge in your existing OpenShift application, go to **"See the list of cartridges you can add"**, paste the URL below in **"Install your own cartridge"** textbox at the bottom of the page and click "Next".

    http://cartreflect-claytondev.rhcloud.com/github/icflorescu/openshift-cartridge-mongodb

## Notes

- Can't guarantee this cartridge is production-ready. Some people use it though (on **their own responsibility**).
- This is a lean cartridge. To save space, just `mongod` is installed. No client libraries, no `mongo` console. If you need external access to your data, use `rhc port-forward`.
- Can't think of a way to make this cartridge auto-updatable. There's nothing like [semver.io](http://semver.io) for MongoDB. For now we'll just have to use `mongodump`, destroy the cartridge, install the new version, then do a `mongorestore`.
- Don't hesitate to make a pull-request with an updated version in [this file](https://github.com/icflorescu/openshift-cartridge-nodejs/blob/master/metadata/manifest.yml#L4) if you notice this cartridge version is behind  the latest stable [official MongoDB linux binary](http://www.mongodb.org/downloads).

## Related

Since you're here, chances are you might also be interested in this [custom Node.js cartridge](https://github.com/icflorescu/openshift-cartridge-nodejs).

## License

The [MIT License](http://github.com/icflorescu/openshift-cartridge-nodejs/LICENSE).
