
=========================================================
How to download and set up the DAWN developer environment
=========================================================

Prerequisite
============
First, you need to download and install the latest Eclipse IDE for RCP and RAP developers 
package available at the following link:

http://www.eclipse.org/downloads/

Developers links
================
The following explain how a developer can check out and start working on Dawn, without 
requiring access to Diamond (i.e. no FedID needed).
For users, separate instructions (the download location) apply.

Resources:

Infrastructure Guide (IG):

http://www.opengda.org/documentation/manuals/Infrastructure_Guide/trunk/contents.html

Reference Card (RC):

http://www.opengda.org/documentation/manuals/Reference_Card/trunk/reference.html

Materialize through Buckminster
===============================
**1.** Install Buckminster (used to manage the checkout from various repositories)

* When you initially start working on Dawn, you will need to install Buckminster into the Eclipse IDE that you use for development. Follow IG 1.2 to find out how to install Buckminster.

* You may also install a headless (command line) version Buckminster. This is optional, but useful. 
Follow IG 1.3 to find out how to install Buckminster headless (documentation currently incomplete).

**2.** Set up a new workspace, with an empty target platform (this is NOT optional!)

* Follow IG 3.1.2 (simplest) or IG 3.1.3 (a bit more manual)
* There is a command line tool to do this, not currently published externally

**3.** Check out the source code for Dawn from the various repositories (Buckminster calls this "materializing").

* On the RC, look up the details for what you want to check out. For Dawn 1.0, you want dawn-v1.0.cquery with component org.dawnsci.base.site.
* Follow IG 3.2.1 to materialize the source code. Note the following changes:
On the Component Query "Properties" tab, set the "download.location" to "public".
If you are not registered with GitHub, change the "github.authentication" type to "anonymous" – 
though this means that you will not be able to check in any changes you make.

**4.** Do your usual developer thing, change code, build the workspace, etc.

**5.** To create the stand-alone product, make sure everything can build ok.
 
* Right-click the org.dawnsci.base.site project,
* Select Buckminster > Invoke Action Set Properties File: to buckminster.properties in the org.dawnsci.base.site project 
* Select one or more create.product-<platform> actions, and 
* Click OK The resulting products(s) will be in /tmp/org.dawnsci.base.site/output/*site*/ (cd to this, and tab-complete to expand the "*site*") Each product is in a directory called dawn.<platform>

