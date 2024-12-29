# `xenomorph` —  Shapeshift between package formats

A tool that converts software packages to work from one package manager to the next,
originating as a rewrite of the classic [`alien`](https://sourceforge.net/projects/alien-pkg-convert/) script.

Currently, the tool supports converting between:
 - `.deb` packages — used by `dpkg`, prevalent in Linux distributions (distros)
   derived from Debian and Ubuntu;
 - `.rpm` packages — used by `rpm`, found in Red Hat-derived distros such as RHEL,
   CentOS, openSUSE, Fedora and more;
 - LSB packages — used by Linux Standard Base and are basicaly `.rpm` packages
 - `.tgz` packages — used by Slackware Linux
 - `.pkg` packages — used by Solaris

## How is `xenomorph` different from `alien`?

`xenomorph` is written in Rust and therefore does not rely on a Perl interpreter in order to function,
and can attain native level speeds comparable to tools written in other native languages.
It also has far more robust error handling and avoids many silent failure states that `alien` can face,
as well as a much more extensible and well-documented framework for other packaging formats.

`xenomorph`, however, *does not* aim to be a complete drop-in replacement of `alien`. Notably,
support for `.slp` packages — once used by Stampede Linux — has been removed, since Stampede Linux
had been effectively dead for over twenty years with little surviving records of its package format. 
`xenomorph`'s CLI interface, while currently compatible with `alien`, may change in the future,
and so will the packages it generates.

## How to build `xenomorph`

To build `xenomorph`, you need to install the RUST toolchain.
For example, on a Debian or derivative (like Ubuntu), use:
> sudo apt install cargo

Note: in order to fix build dependencies on Ubuntu 22.04, you will to:
> cargo update home@0.5.11 --precise 0.5.9

Now, to actually build `xenomorph`, simply use:
> cargo build --release

And to launch it, use:
> ./target/release/xenomorph

## How to use `xenomorph`

> Usage: xenomorph [options] file [...]

To get additional help, refer to:
> xenomorph -h

## How to build `xenomorph` Debian package using cargo-deb

Additionally, if you want to package `xenomorph` as a .deb using cargo-deb, first install cargo-deb using:
> cargo install cargo-deb

Then build the .deb using:
> cargo deb

Note: before running cargo-deb, don't forget to add ~/.cargo/bin to your PATH, for example using:
> export PATH=$PATH:~/.cargo/bin

You can then install the package using:
> sudo dpkg -i target/debian/xenomorph*.deb

## Known Issues

 - Names need to be mapped from `.rpm` to `.deb` - in particular, `.deb` package
       names cannot contain uppercase letters, whereas `.rpm` packages have no such restriction.
	   
 - Currently dependencies from `.deb` files are not processed, which means `.rpm`
	   packages converted from `.deb` packages may not install correctly.

As well as issues that have been carried over from `alien`:

 - Relocatable conffiles, partially relocatable packages, and multipart packages are not yet supported

 - RPM ghost files are not yet supported

 - In Slackware packages, descriptions in install/slack-desc may be ignored

## License

`xenomorph` is licensed under [the GNU General Public License, version 2](LICENSE), or (at your option) any later version.

© 2023–2024 Leah Amelia "pluie" Chen
