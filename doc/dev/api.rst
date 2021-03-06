==============
 Flycheck API
==============

.. require:: flycheck

This chapter provides a brief overview over the Flycheck API.

You may use this API to extend Flycheck, e.g. by implementing new error parsers
or more in-depth error analysis.  You will also find this API helpful if you
want to develop Flycheck itself.

.. _error-api:

Error API
=========

Flycheck errors are represented by the CL structure :cl-struct:`flycheck-error`.
See :infonode:`(cl)Structures` for more information about CL structures.

.. cl-struct:: flycheck-error

   A Flycheck error with the following slots.  Each of these slots may be `nil`.

   .. cl-slot:: buffer

      The buffer object referring to the buffer this error belongs to.

      .. note::

         You do not need to set this attribute when creating errors in an error
         parser.  Flycheck automatically keeps track of the buffer itself.

   .. cl-slot:: checker

      The syntax checker that reported this error.

   .. cl-slot:: filename

      A string containing the filename the error refers to.

   .. cl-slot:: line

      An integer providing the line the error refers to.

   .. cl-slot:: column

      An integer providing the column the error refers to.

      If this attribute is `nil`, Flycheck will assume that the error refers to
      the whole line.

   .. cl-slot:: message

      The human-readable error message as string.

   .. cl-slot:: level

      The error level of the message, as symbol denoting an error level defined
      with :function:`flycheck-define-error-level`.

   There are two constructors to create new :cl-struct:`flycheck-error` objects:

   .. function:: flycheck-error-new-at line column &optional level message &key \
                    checker filename buffer

      Create a new Flycheck error at the given :var:`line` and :var:`column`.

      :var:`line` and :var:`column` refer to the :cl-slot:`line` and
      :cl-slot:`column` of the new error.  The optional :var:`level` and
      :var:`message` arguments fill the :cl-slot:`level` and cl-slot:`message`
      slots respectively.

      :var:`checker`, :var:`filename` and :var:`buffer` are keyword arguments,
      for :cl-slot:`checker`, :cl-slot:`filename` and :cl-slot:`buffer`
      respectively.  :var:`buffer` defaults to the current buffer, the other two
      default to `nil`.

      .. warning::

         Due to a limitation of Common Lisp functions in Emacs Lisp, you must
         specify **all** optional arguments, that is, **both** :var:`level`
         **and** :var:`message`, to pass any keyword arguments.

   .. function:: flycheck-error-new &rest attributes

      Create a new :cl-struct:`flycheck-error` with the given :var:`attributes`.

      :var:`attributes` is a property list, where each property specifies the
      value for the corresponding slot of :cl-struct:`flycheck-error`, for
      instance:

      .. code-block:: cl

         (flycheck-error-new :line 10 :column 5 :message "Foo" :level 'warning)

   The following functions and macros work on errors:

   .. macro:: flycheck-error-with-buffer
      :auto:

   .. function:: flycheck-error-line-region
      :auto:

   .. function:: flycheck-error-column-region
      :auto:

   .. function:: flycheck-error-thing-region
      :auto:

   .. function:: flycheck-error-pos
      :auto:

   .. function:: flycheck-error-format
      :auto:

The following functions and variables may be used to analyze the errors of a
syntax check.

.. variable:: flycheck-current-errors
   :auto:

.. function:: flycheck-count-errors
   :auto:

.. function:: flycheck-has-errors-p
   :auto:

.. _builtin-error-parsers:

Builtin error parsers
=====================

.. function:: flycheck-parse-with-patterns
   :auto:

.. function:: flycheck-parse-checkstyle
   :auto:

.. _error-parser-api:

Error parser API
================

These functions can be used to implement custom error parsers:

.. function:: flycheck-parse-xml-string
   :auto:

.. _syntax-checker-api:

Syntax checker API
==================

.. function:: flycheck-registered-checker-p
   :auto:

.. function:: flycheck-substitute-argument
   :auto:

.. function:: flycheck-locate-config-file
   :auto:

.. function:: flycheck-define-error-level
   :auto:

.. _builtin-option-filters:

Builtin option filters
======================

.. function:: flycheck-option-int
   :auto:

.. function:: flycheck-option-comma-separated-list
   :auto:

Utilities
=========

.. function:: flycheck-rx-to-string
   :auto:

.. function:: flycheck-string-list-p
   :auto:

.. function:: flycheck-symbol-list-p
   :auto:
