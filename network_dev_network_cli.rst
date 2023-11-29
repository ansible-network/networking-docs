********************************
Developing network_cli platforms
********************************

.. contents:: Topics

Updated: 27-Nov-2017

Overview
==========

The document provides details on how to implement a module using the new
network_cli connection plugin.  This document exists in the ``ansible/community`` namespace to allow quicker updates based on feedback from Partners. Once the documentation is stable it will be moved to ``docs/docsite/rst/dev_guide`` in the main Ansible GitHub repository.

Starting in Ansible 2.5 ``network_cli`` is a new first-class connection plugin to replace the ``connection: local``  functionality that was available in Ansible 2.2 through 2.4.

As the Ansible network team begins the transition towards implementing the
network_cli connection plugin, we wanted to provide a sample implementation
for module developers to use as a guide.

Requirements for network module developers
==========================================

Writing a module to support the use of the network_cli connection plugin is
quite a bit easier than the current effort necessary to support connection
local.  This example will touch on the three key pieces necessary to provide
support for a new network platform (operating system) in order to develop
modules that implement a network_cli connection.

This document will guide you through the files used to provide support to a
Cisco IOS device and shows the necessary code required to support the
platform using network_cli.

Components of network_cli
==========================

Terminal Plugin
---------------

The first thing that must be developed is a terminal plugin.  Terminal plugins
are maintained at
`plugins/terminal/ <https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/terminal>`_

The purpose of the terminal plugin is to hook the operating system to set up
the terminal environment.  The terminal plugin is responsible for providing the
list of terminal prompts to look for as well as authorize CLI sessions.  For
instance, switch to enable mode on a Cisco IOS device.

You can see the IOS terminal plugin `plugins/terminal/ios.py <https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/terminal/ios.py>`_

To support a new platform, the terminal plugin should be created and, at a
minimum, the instance variables ``terminal_stdout_re`` and ``terminal_stderr_re``
should be provided.  These instance variables are used to introspect the
response stream after a command is sent to determine if the stream has been
returned and/or if an error as been generated.

cliconf Plugin
--------------

Once the terminal plugin has been created, its time to move on to implementing
the platform cliconf plugin.  The cliconf plugin provides the basic set of
functions to be executed on the device.

Cliconf plugins are maintained at `plugins/cliconf/ <https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/cliconf>`_

The purpose of the cliconf plugin is to implement standardized calls for
platform features such as retrieving the output from commands and editing the
platform configuration.
