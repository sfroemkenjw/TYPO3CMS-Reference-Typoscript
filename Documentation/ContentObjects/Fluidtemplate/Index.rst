.. include:: /Includes.rst.txt
.. index::
   Content objects; FLUIDTEMPLATE
   FLUIDTEMPLATE
.. _cobj-fluidtemplate:
.. _cobj-template:

=============
FLUIDTEMPLATE
=============

An object of type FLUIDTEMPLATE combines TypoScript with the Fluid
templating engine.

.. versionchanged:: 11.0
   The content object FLUIDTEMPLATE has replaced `TEMPLATE
   <https://docs.typo3.org/m/typo3/reference-typoscript/10.4/en-us/ContentObjects/Template/Index.html>`__
   which was removed with version 11.0.

FLUIDTEMPLATE generates content using Fluid templates.
It can be used in :ref:`content elements <t3coreapi:adding-your-own-content-elements>`
or to generate content within the top-level object page
(see :ref:`the example on this page <cobj-fluidtemplate-examples>`).

Data available in Fluid templates
=================================

The following data will be available in the called Fluid template:

*  The content of the current :php:`data` array.

   *  On page level it contains the current page record.
   *  If the :typoscript:`FLUIDTEMPLATE` is used in the
      context of the Fluid ViewHelper :html:`<f:cObject>` it contains the data set
      in the Fluid Property :ref:`data <t3viewhelper:cobject_data>`.
   *  If called in the context of Extbase it contains the data assigned to the view
      in the :ref:`Controller <t3coreapi:extbase-action-controller>`.

*  The :php:`settings` array set by the parameter
   :ref:`settings <cobj-fluidtemplate-properties-settings>`
*  Variables set by the setting
   :ref:`variables <cobj-fluidtemplate-properties-variables>`
*  Additional data retrieved by
   :ref:`data processors <cobj-fluidtemplate-properties-dataprocessing>`

You can use the ViewHelper :ref:`debug <t3viewhelper:typo3fluid-fluid-debug>` to
receive a complete listing of the available data using the magic `{_all}` variable:

.. code-block:: html

   <f:debug>{_all}</f:debug>

.. contents:: On this page:
   :local:
   :depth: 1

.. toctree::
   :hidden:

   DataProcessing

.. index:: FLUIDTEMPLATE; Properties
.. _cobj-fluidtemplate-properties:

Properties
==========

.. _fluidtemplate-dataProcessing:

dataProcessing
--------------

..  t3-cobj-fluidtemplate:: dataProcessing

    :Data type: array of class references by full namespace

    Add one or multiple processors to manipulate the :php:`$data` variable of
    the currently rendered content object, like tt_content or page. The sub-
    property :typoscript:`options` can be used to pass parameters to the processor
    class.

    ..  note::

        The content was moved to the subpage :ref:`dataProcessing`.

.. _cobj-fluidtemplate-properties-extbase-controlleractionname:

extbase.controllerActionName
----------------------------

..  t3-cobj-fluidtemplate:: extbase.controllerActionName

    :Data type: string /:ref:`stdWrap <stdwrap>`

    Sets the name of the action.

.. _cobj-fluidtemplate-properties-extbase-controllerextensionname:

extbase.controllerExtensionName
-------------------------------

..  t3-cobj-fluidtemplate:: extbase.controllerExtensionName

    :Data type: string /:ref:`stdWrap <stdwrap>`

    Sets the extension name of the controller.

    ..  tip::
        Setting this allows you to skip the `extensionName` argument for the
        :ref:`f:translate <t3viewhelper:typo3-fluid-translate>` and
        the :ref:`f:uri.resource <t3viewhelper:typo3-fluid-uri-resource>` ViewHelpers.

        This requires you to put translations and public images in the
        :ref:`usual paths in your extension <t3coreapi:extension-reserved-folders>`.

.. _cobj-fluidtemplate-properties-extbase-controllername:

extbase.controllerName
----------------------

..  t3-cobj-fluidtemplate:: extbase.controllerName

    :Data type: string /:ref:`stdWrap <stdwrap>`

    Sets the name of the controller.

.. _cobj-fluidtemplate-properties-extbase-pluginname:

extbase.pluginName
------------------

..  t3-cobj-fluidtemplate:: extbase.pluginName

    :Data type: string /:ref:`stdWrap <stdwrap>`

    Sets variables for initializing extbase.

.. _cobj-fluidtemplate-properties-file:

file
----

..  t3-cobj-fluidtemplate:: file

    :Data type: string /:ref:`stdWrap <stdwrap>`

    The fluid template file. It is an alternative to ".template" and is used
    only, if ".template" is not set.

    **Example:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page = PAGE
        page {
           10 = FLUIDTEMPLATE
           10 {
              file = EXT:site_package/Resources/Private/Templates/Page/MyTemplate.html
           }
        }

.. _cobj-fluidtemplate-properties-format:

format
------

..  t3-cobj-fluidtemplate:: format

    :Data type: keyword /:ref:`stdWrap <stdwrap>`
    :Default: html

    :typoscript:`format` sets the format of the current request. It can be something
    like "html", "xml", "png", "json". And it can even come in the form of
    "rss.xml" or alike.

.. _cobj-fluidtemplate-properties-layoutrootpath:

layoutRootPath
--------------

..  t3-cobj-fluidtemplate:: layoutRootPath

    :Data type: file path /:ref:`stdWrap <stdwrap>`

    Sets a specific layout path, usually
    :file:`EXT:some_extension/Resources/Private/Layouts/` or a folder below that
    path.

    ..  note::
        It is recommended to use
        :ref:`cobj-fluidtemplate-properties-layoutrootpaths` (mind the
        plural "s") as it can be easily extended by custom templates provided
        by the sitepackage.

.. _cobj-fluidtemplate-properties-layoutrootpaths:

layoutRootPaths
---------------

..  t3-cobj-fluidtemplate:: layoutRootPaths

    :Data type: array of file paths with :ref:`stdWrap <stdwrap>`

    .. note:: Mind the plural -s in "layoutRootPaths"!

    Used to define several paths for layouts, which will be tried in reversed
    order (the paths are searched from bottom to top). The first folder where
    the desired layout is found, is used. If the array keys are numeric, they
    are first sorted and then tried in reversed order.

    **Example:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page {
           10 = FLUIDTEMPLATE
           10 {
              file = EXT:site_default/Resources/Private/Templates/Main.html
              layoutRootPaths {
                 10 = EXT:site_default/Resources/Private/Layouts
                 20 = EXT:site_modification/Resources/Private/Layouts
              }
           }
        }

    If property
    :ref:`layoutRootPath <cobj-fluidtemplate-properties-layoutrootpath>`
    (singular) is also used, it will be placed as the first option
    in the list of fall back paths.

.. _cobj-fluidtemplate-properties-partialrootpath:

partialRootPath
---------------

..  t3-cobj-fluidtemplate:: partialRootPath

    :Data type: file path /:ref:`stdWrap <stdwrap>`

    Sets a specific partial path, usually
    :file:`EXT:some_extension/Resources/Private/Partials/` or a folder below that
    path.

    ..  note::
        It is recommended to use
        :ref:`cobj-fluidtemplate-properties-partialrootpaths` (mind the
        plural "s") as it can be easily extended by custom templates provided
        by the sitepackage.

.. _cobj-fluidtemplate-properties-partialrootpaths:

partialRootPaths
----------------

..  t3-cobj-fluidtemplate:: partialRootPaths

    :Data type: array of file paths with :ref:`stdWrap <stdwrap>`

    .. note:: Mind the plural -s in "partialRootPaths"!

    Used to define several paths for partials, which will be tried in reversed
    order. The first folder where the desired partial is found, is used. The
    keys of the array define the order.

    See :ref:`layoutRootPaths <cobj-fluidtemplate-properties-layoutrootpaths>`
    for more details.

.. _cobj-fluidtemplate-properties-settings:

settings
--------

..  t3-cobj-fluidtemplate:: settings

    :Data type: array of keys

    Sets the given settings array in the fluid template. In the view, the value
    can then be used.

    **Example:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page = PAGE
        page {
           10 = FLUIDTEMPLATE
           10 {
              file = EXT:site_default/Resources/Private/Templates/MyTemplate.html
              settings {
                 copyrightYear = 2013
              }
           }
        }

    To access copyrightYear in the template file use this:

    ..  code-block:: html

        {settings.copyrightYear}

    Apart from setting a key-value pair as done in the example, you can
    also reference objects or access constants as well.

.. _cobj-fluidtemplate-properties-stdwrap:

stdWrap
-------

..  t3-cobj-fluidtemplate:: stdWrap

    :Data type: :ref:`->stdWrap <stdwrap>`

    Offers the usual stdWrap functionality.

.. _cobj-fluidtemplate-properties-template:

template
--------

..  t3-cobj-fluidtemplate:: template

    :Data type:  :ref:`cObject <data-type-cobject>`

    Use this property to define a content object, which should be used as
    template file. It is an alternative to ".file"; if ".template" is set, it
    takes precedence.

    ..  warning::

        The :typoscript:`FILE` object type has been removed in TYPO3 v10. As the :typoscript:`.template`
        property used :typoscript:`FILE`, you should generally check your code if
        using this and switch to using :ref:`.templateName <cobj-fluidtemplate-properties-templatename>`
        with :ref:`.templateRootPaths <cobj-fluidtemplate-properties-templaterootpaths>` or use
        :ref:`.file <cobj-fluidtemplate-properties-file>`.

.. _cobj-fluidtemplate-properties-templatename:

templateName
------------

..  t3-cobj-fluidtemplate:: templateName

    :Data type: string /:ref:`stdWrap <stdwrap>`

    This name is used together with the set format to find the template in the
    given templateRootPaths. Use this property to define a content object, which
    should be used as template file. It is an alternative to :typoscript:`.file`. If

    `.templateName` is set, it takes precedence.

    **Example 1:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        lib.stdContent = FLUIDTEMPLATE
        lib.stdContent {
           templateName = Default
           layoutRootPaths {
              10 = EXT:frontend/Resources/Private/Layouts
              20 = EXT:sitemodification/Resources/Private/Layouts
           }
           partialRootPaths {
              10 = EXT:frontend/Resources/Private/Partials
              20 = EXT:sitemodification/Resources/Private/Partials
           }
           templateRootPaths {
              10 = EXT:frontend/Resources/Private/Templates
              20 = EXT:sitemodification/Resources/Private/Templates
           }
           variables {
              foo = TEXT
              foo.value = bar
           }
        }

    **Example 2:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        lib.stdContent = FLUIDTEMPLATE
        lib.stdContent {
           templateName = TEXT
           templateName.stdWrap {
              cObject = TEXT
              cObject {
                 data = levelfield:-2,backend_layout_next_level,slide
                 override.field = backend_layout
                 split {
                    token = frontend__
                    1.current = 1
                    1.wrap = |
                 }
              }
              ifEmpty = Default
           }
           layoutRootPaths {
              10 = EXT:frontend/Resources/Private/Layouts
              20 = EXT:sitemodification/Resources/Private/Layouts
           }
           partialRootPaths {
              10 = EXT:frontend/Resources/Private/Partials
              20 = EXT:sitemodification/Resources/Private/Partials
           }
           templateRootPaths {
              10 = EXT:frontend/Resources/Private/Templates
              20 = EXT:sitemodification/Resources/Private/Templates
           }
           variables {
              foo = bar
           }
        }

.. _cobj-fluidtemplate-properties-templaterootpath:

templateRootPath
----------------

..  t3-cobj-fluidtemplate:: templateRootPath

    :Data type: file path /:ref:`stdWrap <stdwrap>`

    Sets a specific template path, usually
    :file:`EXT:some_extension/Resources/Private/Templates/` or a folder below that
    path.

    ..  note::
        It is recommended to use
        :ref:`cobj-fluidtemplate-properties-templaterootpaths` (mind the
        plural "s") as it can be easily extended by custom templates provided
        by the sitepackage.

.. _cobj-fluidtemplate-properties-templaterootpaths:

templateRootPaths
-----------------

..  t3-cobj-fluidtemplate:: templateRootPaths

    :Data type: array of file paths with :ref:`stdWrap <stdwrap>`

    .. note:: Mind the plural -s in "templateRootPaths"!

    Used to define several paths for templates, which will be tried in reversed
    order (the paths are searched from bottom to top). The first folder where
    the desired layout is found, is used. If the array keys are numeric, they
    are first sorted and then tried in reversed order.

    Useful in combination with the
    :ref:`templateName <cobj-fluidtemplate-properties-templatename>` property.

    **Example:**

    ..  code-block:: typoscript
        :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

        page {
           10 = FLUIDTEMPLATE
           10 {
              templateName = Default
              templateRootPaths {
                 10 = EXT:sitedesign/Resources/Private/Templates
                 20 = EXT:sitemodification/Resources/Private/Templates
              }
           }
        }

.. _cobj-fluidtemplate-properties-variables:

variables
---------

..  t3-cobj-fluidtemplate:: variables

    :Data type: *(array of cObjects)*

    Sets variables that should be available in the fluid template. The keys are
    the variable names in Fluid.

    Reserved variables are "data" and "current", which are filled automatically
    with the current data set.

.. _cobj-fluidtemplate-examples:

Example
=======

The Fluid template in
:file:`EXT:site_default/Resources/Private/Templates/MyTemplate.html` could look
like this:

..  code-block:: html

    <h1>{data.title}<f:if condition="{data.subtitle}">, {data.subtitle}</f:if></h1>
    <h3>{mylabel}</h3>
    <f:format.html>{data.bodytext}</f:format.html>
    <p>&copy; {settings.copyrightYear}</p>

You could use it with a TypoScript code like this:

..  code-block:: typoscript

    page = PAGE
    page.10 = FLUIDTEMPLATE
    page.10 {
       templateName = MyTemplate
       templateRootPaths {
          10 = EXT:site_default/Resources/Private/Templates
       }
       partialRootPaths {
          10 = EXT:site_default/Resources/Private/Partials
       }
       variables {
          mylabel = TEXT
          mylabel.value = Label coming from TypoScript!
       }
       settings {
          # Get the copyright year from a TypoScript constant.
          copyrightYear = {$year}
       }
    }

As a result the page title and the label from TypoScript will be inserted as
headlines. The copyright year will be taken from the TypoScript constant
"year".

..  seealso::

    *   :ref:`dataProcessing` examples
    *   :ref:`t3coreapi:adding-your-own-content-elements`
    *   :ref:`t3sitepackage:start`
