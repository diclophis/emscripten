.. _emsdk:

=====================================================
Emscripten SDK Manager (emsdk) (ready-for-review)
=====================================================

**The Emscripten SDK management script (** ``emsdk`` **) is used to perform all SDK maintenance. You only need to "install" the SDK once; after that emsdk can do all further updates!**

Purpose
============================================

With *emsdk* you can download, install or remove *any* :term:`SDK` or :term:`Tool`, ranging from the very first, through to the :ref:`bleeding edge updates <emsdk-master-or-incoming-sdk>` still on Github. Most operations are of the form ``emsdk command``. To access the *emsdk*, launch the :ref:`Emscripten Command Prompt <emcmdprompt>`.

This document provides the command syntax, and a :ref:`set of guides <emsdk_howto>` explaining how to perform both common and advanced maintenance operations.


Command line syntax
============================================

**emsdk** [**help** [**--old**] | **list** | **update** | **install** *<tool/sdk>* | **uninstall** *<tool/sdk>* | **activate** *<tool/sdk>*]


Arguments
---------
 

.. list-table:: 
   :header-rows: 1
   :widths: 20 80
   :class: wrap-table-content 

   * - Command
     - Description
   * - ``list [--old]``
     - Lists all current SDKs and tools and their installation status. With the ``--old`` parameter, historical versions are also shown.
   * - ``update``
     - Fetches the latest list of all available tools and SDKs (but does not install them)
   * - ``install <tool/sdk>``
     - Downloads and installs the :ref:`specified tool or SDK <emsdk-specified-tool-sdk>`.
   * - ``uninstall <tool/sdk>``
     - Removes the :ref:`specified tool or SDK <emsdk-specified-tool-sdk>` from the disk.
   * - ``activate <tool/sdk>``
     - Sets the :ref:`specified tool or SDK <emsdk-specified-tool-sdk>` as the default tool in the system environment.
   * - ``help``
     - Lists all supported commands. The same list is output if no command is specified.	 

.. note:: For Mac OSX the commands are called with  **./emsdk**  and **./emcmdprompt.bat** respectively.

Note that **emcmdprompt.bat** is also displayed as an option in the ``emsdk help``. This is not intended to be called through the command line. See :ref:`emcmdprompt` for more information.


.. _emsdk-specified-tool-sdk:

Tools and SDK targets
------------------------
	 
The ``<tool/sdk>`` given above as a command argument is one of the targets listed using ``emsdk list`` (or ``emsdk list --old``). 

Note that some of the tools and SDK names include  *master* or *incoming*: these targets are used to clone and pull the very latest versions from the Emscripten incoming and master branches.

Finally, you can specify a target of ``latest`` to grab the most current SDK.

.. todo:: **HamishW**  emcmdprompt.bat does not appear to work. Need to check with Jukka



SDK manager concepts
==============================

The SDK contains a number of different :term:`tools <Tool>`, including *Clang*, *Emscripten*, *Java*, *Git*, *Node*, etc. The *emsdk* can fetch the different versions of all these tools and also specific SDKs. These are put into different directories under the main SDK installation folder, grouped by tool and version. 

A user-specific file (**~/.emscripten**) stores the active "compiler configuration"; the :term:`active <Active Tool/SDK>` configuration is the specific set of tools that are used by default if Emscripten in called on the :ref:`Emscripten Command Prompt <emcmdprompt>`. Users can call *emsdk* with the ``activate`` argument to make a specific tool or SDK active.


.. _emsdk_howto:

"How to" guides
=========================

The following topics explain how to perform both common and advanced maintenance operations, ranging from installing the latest SDK through to installing your own fork from Github.

.. note:: The examples below show the commands for Windows and Linux. The commands are the same on Mac OSX, but you need to replace **emsdk** with **./emsdk**.

.. _emsdk-get-latest-sdk:

How do I just get the latest SDK?
------------------------------------------------------------------------------------------------
Use the ``update`` argument to fetch the current registry of available tools, and then the ``latest`` target to get the most recent SDK: ::

	# Fetch the latest registry of available tools.
	emsdk update

	# Download and install the latest SDK tools.
	emsdk install latest
	
	# Set up the compiler configuration to point to the "latest" SDK.
	emsdk activate latest	



How do I use emsdk?
--------------------------------

Use ``emsdk help`` or just ``emsdk`` to get information about all available commands.

	
How do I check which versions of the SDK and tools are installed?
------------------------------------------------------------------------------------------------

To get a list of all currently installed tools and SDK versions (and all available tools) run: ::

	emsdk list

A line will be printed for each tool/SDK that is available for installation. The text ``INSTALLED`` will be shown for each tool that has already been installed. If a tool/SDK is currently active, a star (\*) will be shown next to it. 

	
How do I install a tool/SDK version?
------------------------------------

Use the ``install`` argument to download and install a new tool or an SDK version: ::

	emsdk install <tool/sdk name>

	
.. _emsdk-remove-tool-sdk:
	
How do I remove a tool or an SDK?
----------------------------------------------------------------

Use the ``uninstall`` argument to delete a given tool or SDK from the local computer: ::

	emsdk uninstall <tool/sdk name>
	

See :ref:`downloads-uninstall-the-sdk` if you need to completely remove Emscripten from your system. 



	
How do I check for updates to the Emscripten SDK?
----------------------------------------------------------------

First use the ``update`` command to fetch package information for all new tools and SDK versions. Then use ``install <tool/sdk name>`` to install a new version: ::

	# Fetch the latest registry of available tools.
	emsdk update
	
	# Download and install the specified new version.
	emsdk install <tool/sdk name> 	


How do I change the currently active SDK version?
----------------------------------------------------------------

Toggle between different tools and SDK versions using the :term:`activate <Active Tool/SDK>` command. This will set up ``~/.emscripten`` to point to that particular tool: ::

	emsdk activate <tool/sdk name>
	
	
How do I install an old Emscripten compiler version?
----------------------------------------------------------------

*Emsdk* contains a history of old compiler versions that you can use to maintain your migration path. Use the ``list --old`` argument to get a list of archived tool and SDK versions, and ``install <name_of_tool>`` to install it: ::

	emsdk list --old
	emsdk install <name_of_tool>
	
On Windows, you can directly install an old SDK version by using one of the :ref:`archived offline NSIS installers <archived-nsis-windows-sdk-releases>`. 



.. _emsdk-master-or-incoming-sdk:

How do I track the latest Emscripten development with the SDK?
------------------------------------------------------------------------------------------------

It is also possible to use the latest and greatest versions of the tools on the Github repositories! This allows you to obtain new features and latest fixes immediately as they are pushed to Github, without having to wait for release to be tagged. **No Github account or a fork of Emscripten is required.** 

To switch to using the latest upstream git development branch ``incoming``, run the following:

::

	# Install git. Skip if the system already has it.
	emsdk install git-1.8.3 
	
	# Clone+pull the latest kripken/emscripten/incoming.
	emsdk install sdk-incoming-64bit
	
	# Set the incoming SDK as the active.
	emsdk activate sdk-incoming-64bit 	

If you want to use the upstream stable branch ``master``, then replace ``-incoming-`` with ``-master-`` above.

.. note:: On Windows, *git* may fail with the error message: 

	::

		Unable to find remote helper for 'https' when cloning a repository with https:// url. 
		
	The workaround is to uninstall git from *emsdk* (``emsdk uninstall git-1.8.3``)  and install `Git for Windows <http://msysgit.github.io>`_. This issue is reported `here <https://github.com/juj/emsdk/issues/13>`_.
	
.. todo:: **HamishW** Check whether the bug (https://github.com/juj/emsdk/issues/13) is fixed and remove the above note if it is.

	
How do I use my own Emscripten Github fork with the SDK?
----------------------------------------------------------------

It is also possible to use your own fork of the Emscripten repository via the SDK. This is useful in the case when you want to make your own modifications to the Emscripten toolchain, but still keep using the SDK environment and tools.

The way this works is that you first install the ``sdk-incoming`` SDK as in the :ref:`previous section <emsdk-master-or-incoming-sdk>`. Then you use familiar git commands to replace this branch with the information from your own fork:

::

	cd emscripten/incoming
	
	# Add a git remote link to your own repository.
	git remote add myremote https://github.com/mygituseraccount/emscripten.git
	
	# Obtain the changes in your link.
	git fetch myremote
	
	# Switch the emscripten-incoming tool to use your fork.
	git checkout -b myincoming --track myremote/incoming

You can switch back and forth between remotes via the ``git checkout`` command as usual.




