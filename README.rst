Varnish module collection by Varnish Software
=============================================

This is a collection of modules ("vmods") extending Varnish VCL used for
describing HTTP request/response policies with additional capabilities.

Included:

* Simpler handling of HTTP cookies
* Variable support
* Request and bandwidth throttling
* Modify and change complex HTTP headers
* 3.0-style saint mode,
* Advanced cache invalidations, and more.

This collection contains the following vmods (previously kept individually):
cookie, vsthrottle, header, saintmode, softpurge, tcp, var, xkey

Supported Varnish version is described in the `CHANGES.rst` file. Normally this
is the last public Varnish Cache release. See PORTING below for information on
support for other versions of Varnish.


Installation
------------

Source releases can be downloaded from:

    https://download.varnish-software.com/varnish-modules/


Installation requires an installed version of Varnish Cache, including the
development files.

Source code is built with autotools::

    sudo apt-get install automake          || sudo yum install automake
    sudo apt-get install libtool           || sudo yum install libtool
    sudo apt-get install libvarnishapi-dev || sudo yum install varnish-libs-devel
    ./bootstrap   # If running from git.
    ./configure
    make
    make check   # optional
    sudo make install


The resulting loadable modules (``libvmod_foo*.so`` files) will be installed to
the Varnish module directory. (default `/var/lib/varnish/vmods/`)


Usage
-----

Each module has a different set of functions and usage, described in
separate documents in `docs/`. For completeness, here is a snippet from
`docs/cookie.rst`::

    import cookie;

    sub vcl_recv {
            cookie.parse(req.http.cookie);
            cookie.filter_except("SESSIONID,PHPSESSID");
            set req.http.cookie = cookie.get_string();
            # Only SESSIONID and PHPSESSID are left in req.http.cookie at this point.
    }



Development
-----------

The source git tree lives on Github: https://github.com/varnish/varnish-modules

All source code is placed in the master git branch. Pull requests and issue
reporting are appreciated.

Porting
-------

We encourage porting of the module package to other versions of Varnish Cache.

Nils Goroll has a git tree adopted to Varnish git master, which can be found
on: https://github.com/nigoroll/varnish-modules/



Administrativa
--------------

The goals of this collection are:

* Simplify access to vmod code for Varnish users. One package to install, not 6.
* Decrease the maintenance cost that comes with having 10 different git
  repositories, each with autotools and (previously) distribution packaging files.

Expressed non-goals are:

* Import vmods that require external libraries, like curl or geoip. This
  collection should be simple and maintenance free to run.
* Support older releases of Varnish Cache.
* Include every vmod under the sun. We'll add the important ones.

Addition of further vmods is decided on a case-by-case basis. Code quality and
maintenance requirements will be important in this decision.


Contact
-------

This code is maintained by Varnish Software. (https://www.varnish-software.com/)

Issues can be reported via the Github issue tracker.

Other inquires can be sent to opensource@__no_spam_please__varnish-software.com.

