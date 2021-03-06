Marbaloo ReCaptcha
==================

`Google ReCaptcha <https://www.google.com/recaptcha/intro/index.html>`_ for cherrypy.



Installation
------------
::

    pip install marbaloo_recaptcha

Usage
-----

::

    # app.py
    import cherrypy
    import marbaloo_recaptcha
    cherrypy.tools.recaptcha = marbaloo_recaptcha.Tool()


    class Root(object):

        @cherrypy.expose
        def index(self, **kwargs):
            recaptcha = cherrypy.request.recaptcha
            if 'submit_recaptcha' in kwargs:
                if recaptcha.verify():
                    return 'Success'
                else:
                    return 'Fail'
            else:
                return '''
                <head>
                    %s
                </head>
                <body>
                    <form method="post">
                        %s
                        <input type="submit" name="submit_recaptcha"/>
                    </form>
                </body>
                ''' % (recaptcha.get_html_head(), recaptcha.get_html_field(enable_no_script=True))

    config = {
        'global': {
            "server.socket_host": "0.0.0.0",
            "server.socket_port": 9095,
        },
        '/': {
            'tools.recaptcha.on': True,
            'tools.recaptcha.remoteip': '',
            # For production level, uncomment this lines
            # 'tools.recaptcha.secret_key': 'MY_SECRET_KEY',
            # 'tools.recaptcha.site_key': 'MY_SITE_KEY',
        }
    }
    cherrypy.quickstart(Root(), '/', config)


Advanced Usage:

::

    import cherrypy
    import marbaloo_recaptcha
    cherrypy.tools.recaptcha = marbaloo_recaptcha.Tool()


    class Root(object):

        @cherrypy.expose
        def index(self, **kwargs):
            recaptcha = cherrypy.request.recaptcha
            if 'submit_recaptcha' in kwargs:
                if recaptcha.verify():
                    return 'Success'
                else:
                    return 'Fail'
            else:
                return '''
                <head>
                    %s
                </head>
                <body>
                    <form method="post">
                        %s
                        <input type="submit" name="submit_recaptcha"/>
                    </form>
                </body>
                ''' % (recaptcha.get_html_head(defer=True,
                                               async=True,
                                               render='onload',
                                               onload='my_onload_javascript',
                                               hl='fa'),
                       recaptcha.get_html_field(data_attributes={
                                                                    'data-theme': 'dark',
                                                                    'data-size': 'compact',
                                                                    'data-type': 'image'
                                                                },
                                                enable_no_script=True))

    config = {
        'global': {
            "server.socket_host": "0.0.0.0",
            "server.socket_port": 9095,
        },
        '/': {
            'tools.recaptcha.on': True,
            'tools.recaptcha.remoteip': '',
            # For production level, uncomment this lines
            # 'tools.recaptcha.secret_key': 'MY_SECRET_KEY',
            # 'tools.recaptcha.site_key': 'MY_SITE_KEY',
        }
    }
    cherrypy.quickstart(Root(), '/', config)

