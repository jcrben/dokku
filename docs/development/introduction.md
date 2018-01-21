Several workflows are possible in working on dokku, but this page will describe
common and official practices. The workflows for dokku are not necessarily
optimized for build speed in the way that typical frontend or backend application
development might be. Pull requests to speed up the build are welcome.

### Basic workflow
The simplest way to get started is summarized by this shell script:
```
git clone <repo>
vagrant up
vagrant ssh
sudo cd /vagrant/dokku
```
(TODO: note warning about IP address)
(TODO: figure out why it isn't initializing the hostname and public key)
TODO: figure out what's going on with the Vagrantfile - PREBUILT_STACK_URL? public interface?

At this point, you can switch back to <repo> on the host to edit the files
locally. When you are ready to check your changes, you switch back to the vm 
session and execute: `make copyfiles` to update installed plugins.

After switching back to the host, you can run an integration against one of
apps in <repo>/tests/apps with a command like `make deploy-test-nodejs-express`.
See [Testing](./testing.md) for more on the available tests. 

If none of the example CI apps fit, you can manually test a build and deploy by 
initializing a git repository for your own app, or create another test.

For example, extract and modify one of the CI apps:
```
cp -R <repo>/tests/apps/dockerfile ../
cd ../dockerfile
git init
git remote add app dokku@dokku.me:app
git push app
```

TIP: to get quick information without the compilation delay of make copyfiles,
edit the bash files to log out information

### Advanced workflow
Building and testing smaller pieces of the application in isolation can speed up
development time. If you are implementing an algorithm, writing a failing unit
and iterating until it passes is recommended.
TODO: figure out workflows for building / running plugins in isolation:
trigger plugn inside of a minimal (sort of mocked) non-VM structure and
test that the output changed as expected

### Toolchain
Dokku is built with Go libraries, notably including [plugins](https://github.com/dokku/plugn) and basher.

Many of the plugins are written in bash, but more and more will be written in Go.
TODO: say a bit about this? preconfigured delve in the vagrant box?
TODO: notifications sent to host when 3-minute compilation is complete?

#### Vagrant flow
pretty slow - comment out the make dokku-installer provisioner if possible
https://github.com/fgrehm/vagrant-notify
