Fiole API
=========

.. module:: fiole


Decorators and helpers
----------------------

.. autofunction:: route(url, methods=('GET', 'HEAD'), callback=None, status=200)
.. autofunction:: get(url)
.. autofunction:: post(url)
.. autofunction:: put(url)
.. autofunction:: delete(url)
.. autofunction:: errorhandler(code)

.. autofunction:: send_file(request, filename, root=None, content_type=None, buffer_size=65536)

.. autofunction:: get_template
.. autofunction:: render_template

.. data:: engine

   Default instance of the Template :class:`Engine`.

.. data:: default_app

   Default :class:`Fiole` application.

.. autofunction:: get_app
.. autofunction:: run_fiole(app=None, server=run_wsgiref, host=None, port=None, secret=None)


WSGI application
----------------

.. autoclass:: Fiole

   .. automethod:: push
   .. automethod:: pop
   .. attribute:: static_folder

      Directory where static files are located.  (default: *./static*)

   .. automethod:: handle_request
   .. automethod:: handle_error
   .. automethod:: find_matching_url
   .. automethod:: route
   .. automethod:: get
   .. automethod:: post
   .. automethod:: put
   .. automethod:: delete
   .. automethod:: errorhandler
   .. automethod:: send_file

.. autoclass:: Request

   Environment variables are also accessible through :class:`Request`
   attributes.

   .. attribute:: environ

      Dictionary of environment variables

   .. attribute:: path

      Path of the request, decoded and with ``/`` appended.

   .. attribute:: method

      HTTP method (GET, POST, PUT, ...).

   .. attribute:: query

      Read ``QUERY_STRING`` from the environment.

   .. attribute:: headers

      An instance of :class:`EnvironHeaders` which wraps HTTP headers.

   .. attribute:: content_length

      Header ``"Content-Length"`` of the request as integer or ``0``.

   .. autoattribute:: GET
   .. autoattribute:: POST
   .. autoattribute:: PUT
   .. autoattribute:: body
   .. autoattribute:: cookies

   .. automethod:: get_cookie
   .. automethod:: get_secure_cookie

.. autoclass:: Response

   .. autoattribute:: charset
   .. attribute:: status

      Status code of the response as integer (default: *200*)

   .. attribute:: headers

      Response headers as :class:`HTTPHeaders`.

   .. automethod:: set_cookie
   .. automethod:: clear_cookie
   .. automethod:: set_secure_cookie
   .. automethod:: create_signed_value
   .. automethod:: send

.. autoclass:: HTTPHeaders

   An instance of :class:`HTTPHeaders` is an iterable.  It yields
   tuples ``(header_name, value)``.  Additionally it provides
   a dict-like interface to access or change individual headers.

   .. automethod:: __getitem__

      Access the header by name.  This method is case-insensitive
      and the first matching header is returned.  It returns
      :const:`None` if the header does not exist.

   .. automethod:: get
   .. automethod:: get_all
   .. automethod:: add
   .. automethod:: set
   .. automethod:: setdefault
   .. automethod:: to_list
   .. automethod:: keys
   .. automethod:: values
   .. automethod:: items

.. autoclass:: EnvironHeaders

   .. automethod:: __getitem__

      Access the header by name.  This method is case-insensitive
      and returns :const:`None` if the header does not exist.

   .. automethod:: get
   .. automethod:: get_all
   .. automethod:: keys
   .. automethod:: values
   .. automethod:: items

.. autoexception:: HTTPError
.. autoexception:: BadRequest
.. autoexception:: Forbidden
.. autoexception:: NotFound
.. autoexception:: MethodNotAllowed
.. autoexception:: Redirect
.. autoexception:: InternalServerError


Template engine
---------------

.. autoclass:: Engine

   .. attribute:: global_vars

      This mapping contains additional globals which are injected in the
      generated source.  Two special globals are used internally and must
      not be modified: ``_r`` and ``_i``.  The functions ``str`` and
      ``escape`` are also added here for the ``|s`` and ``|e`` filters.
      They can be replaced by C extensions for performance (see `Webext`_).
      Any object can be added in this registry for usage in the templates,
      either as function or filter.

   .. automethod:: get_template
   .. automethod:: remove(name)
   .. automethod:: import_name
   ..
      render
      compile_template
      compile_import
      load_and_parse

.. autoclass:: Template()

   .. autoattribute:: name

      Name of the template (it can be :const:`None`).

   .. method:: render(context)
               render(**context)

      Render the template with these arguments (either a dictionary
      or keyword arguments).

.. autoclass:: Loader
   :members:

   .. attribute:: template_folder

      Directory where template files are located.  (default: *./templates*)

.. autoclass:: Lexer
   :members:

.. autoclass:: Parser

   .. automethod:: tokenize
   .. automethod:: end_continue
   .. automethod:: parse_iter

.. autoclass:: BlockBuilder

   .. autoattribute:: filters

      A mapping of declared template filters (aliases of globals).  This
      mapping can be extended.  The globals can be extended too, see
      :data:`Engine.global_vars`.

   .. attribute:: rules

      A mapping of ``tokens`` with list of methods, to generate the source
      code.

   .. automethod:: add
   .. automethod:: compile_code

.. _Webext: https://pypi.python.org/pypi/Webext/