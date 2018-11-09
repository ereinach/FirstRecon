Config files for WPDistillery

## Dependencies

- [ScotchBox](https://box.scotch.io) (using [Vagrant](https://vagrantup.com) & [Virtualbox](https://virtualbox.org))
- [Vagrant Hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) (`vagrant plugin install vagrant-hostsupdater`)

## Setup

To setup a new project running Scotch Box and WordPress, simply follow these steps:

1. In a terminal, run: `git clone https://github.com/flurinduerst/WPDistillery.git my-project`
2. Customize `wpdistillery/config.yml` to suit your needs. See [configuration file documentation](README_CONFIG.md).
3. Inside the `Vagrantfile` add your local URL at `config.vm.hostname`. This should be the same as `wpsettings:url:` in `config.yml`.
4. Run `vagrant up` from where your `Vagrantfile` is.

Done! You can now access your project at the local URL (for example `yoursite.vm`) defined in **Step 3**. (or at http://192.168.33.10/)

### 5. Paste these files

I’m putting a lot of time into maintaining WPDistillery, so please consider [donating](https://www.paypal.me/FlurinDuerst/10) or [sharing](https://twitter.com/intent/tweet?url=https%3A%2F%2Fwpdistillery.org). Thank you!
