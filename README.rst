About tgext.webassets
---------------------

tgext.webassets is a TurboGears2 extension for assets management
based on `WebAssets <https://webassets.readthedocs.org/en/latest/index.html>`_.

Installing
----------

tgext.webassets can be installed from pypi::

    pip install tgext.webassets

should just work for most of the users.

Usage
-----

To enable tgext.webassets put inside your application
``config/app_cfg.py`` the following::

    from tgext import webassets

    webassets.plugme(base_config, bundles={
        'js_all': webassets.Bundle('javascript/jquery.js', 'javascript/bootstrap.min.js',
                                   filters='rjsmin', output='assets/js_all.js')
    })

or you can use ``tgext.pluggable`` when available::

    from tgext.pluggable import plug
    from tgext.webassets import Bundle

    plug(base_config, 'tgext.webassets', bundles={
        'js_all': Bundle('javascript/jquery.js', 'javascript/bootstrap.min.js',
                         filters='rjsmin', output='assets/js_all.js')
    })

By default ``tgext.webassets`` will load and save assets inside the turbogears
``static_files`` path. Which is usually the ``public`` directory inside your
application.

.. note::

    Due to ``tgext.webassets`` writing files to the same place where it loads them,
    it is suggested you add a subdirectory to the ``output`` option of your bundles,
    so that the resulting files do not mix with the source files.

The webassets environment will then be available as ``tg.app_globals.webassets``, so you can
use it inside your templates to load the assets::

    <script py:for="asset_url in g.webassets.js_all.urls()" src="$asset_url"></script>

Each registered bundle will be available as a property of the ``g.webassets`` object
inside templates.

Bundles
-------

``ŧgext.webassets`` accepts the bundles inside the ``bundles`` dictionary. Each entry
can be a ``Bundle`` instance or a `webassets loader <https://webassets.readthedocs.org/en/latest/loaders.html>`_
in case you want to load bundles from a configuration file.

Configuring
-----------

Options available in `WebAssets Environment <https://webassets.readthedocs.org/en/latest/environment.html#configuration>`_
are also available in tgext.webassets, pass them as arguments to the ``plug`` (inside the
``options`` dictionary when using ``tgext.webassets.plugme``) or provide the through AppConfig
or *.ini* file with ``webassets.`` namespace.

Some have values that are inherited from your project configuration if not specified:

    * debug: If webassets is running in debug mode or not, by default is ``False``.
    * base_dir: The directory assets will be located. By default is your project ``static_files`` directory.
    * base_url: The url static assets are served, by default ``/``. Use this option to serve them from a CDN.
    * cache: If we should use webassets cache (if boolean), or override default path to cache directory
      by default assets are cached inside the ``.webassets-cache`` directory inside ``base_dir``.

Builtin Filters
---------------

``tgext.webassets`` comes with builtin the ``rjsmin`` and ``cssmin`` filter,
all `webassets filters <https://webassets.readthedocs.org/en/latest/builtin_filters.html>`_
work if the required tools are available.
