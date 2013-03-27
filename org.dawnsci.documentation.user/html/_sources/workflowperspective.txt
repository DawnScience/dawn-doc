Workflow Perspective
====================

The Workflow perspective....

.. image:: images/workflowperspective.png


Maths Actors
++++++++++++

Add
...

An actor to add any data, for instance images, together. The actor has two inputs 'a' and 'b'. Multiple inputs may be connected to both a and b. In this case use the Expression Mode parameter to define the behavior of the actor with the multiple input data. The output is a data set named either with the object name or derived from the input variable 'file_name' depending on the value of the 'Name Mode' attribute.

.. image:: images/add.png

Subtract
........

An actor to subtract one array from another, for instance images. The actor has two inputs 'a' and 'b'. Multiple inputs may be connected to both a and b. In this case use the Expression Mode parameter to define the behavior of the actor with the multiple input data. Note you can repeatedly subtract one image from a pipeline of images by using 'a' for the pipeline and 'b' for the static image. The output is a data set named either with the object name or derived from the input variable 'file_name' depending on the value of the 'Name Mode' attribute.

.. image:: images/subtract.png

Multiply
........

An actor to multiply arrays, for instance images, together. The actor has two inputs 'a' and 'b'. Multiple inputs may be connected to both a and b. In this case use the Expression Mode parameter to define the behavior of the actor with the multiple input data. The output is a data set named either with the object name or derived from the input variable 'file_name' depending on the value of the 'Name Mode' attribute.

.. image:: images/multiply.png

Divide
......

An actor to divide arrays, for instance images, together. The actor has two inputs 'a' and 'b'. Multiple inputs may be connected to both a and b. In this case use the Expression Mode parameter to define the behavior of the actor with the multiple input data. The output is a data set named either with the object name or derived from the input variable 'file_name' depending on the value of the 'Name Mode' attribute.

.. image:: images/divide.png

Median
......

An actor to take a median of arrays, for instance images, together. The actor has two inputs 'a' and 'b'. Multiple inputs may be connected to both a and b. In this case use the Expression Mode parameter to define the behavior of the actor with the multiple input data. The output is a data set named either with the object name or derived from the input variable 'file_name' depending on the value of the 'Name Mode' attribute.

Maths Actor Attributes
......................

* Name - Sets the name used in the workflow. Set to something short or it will truncate (the tooltip shows the full name in this case).
* Expression Mode - Choose between different expression modes. For instance evaluation of the add every time a message is received, once all have been received or caching input to b and completing the add every time a changes.
* Memory Mode - This parameter allows the original data to be modified in memory so save a copy being created. This operation however can have unforeseen downstream effects.
* Name Mode - This attribute is used to define how the output data is named. It either uses the actor name, reads the value of the input variable 'file_name' and tried to assign a data set name from the file or just operates on the data leaving the name unchanged.

Flow Control Actors
+++++++++++++++++++

If
..

This actor allows different paths to be taken in the workflow depending on logical expressions. As an example see the "if_example.moml" workflow in workflows/examples:

.. image:: images/if_example.png

The output port have different names which can be visualised by hoovering the mouse pointer over the ports:

.. image:: images/output_port.png

The different output ports of the if actor are attributed different expressions. These expressions are visualised in the actor attributes view:

.. image:: images/actor_attributes.png

In this example the port 'output' is used if x!=1 and the port 'x==1' is used if x==1. The number of output ports and their expressions can be edited using the 'Create expressions' dialog:

.. image:: images/create_expressions.png

NOTE! There must always be one port called 'output'.


Hardware Actors
+++++++++++++++


Tango Motor
...........

This actor is a transformer type meaning it receives and transmits messages but does not create messages or act as a sink. The actor attaches to a Tango server which provides an attribute for setting value of hardware and reading value. In fact the Tango attribute does not have to move a motor, it can result in any action when the attribute changes.

To set up the Tango connection go to Window->Preferences->Data Analysis->Tango. The system will attempt to automatically set the correct properties on a beamline by reading the environment variables when it starts for the first time. You will likely need to set the spec session name however.

The 'Motors' attribute in the actor optionally sets the motors and always reads their value, passing this downstream in the pipeline. Typically the 'Motor Name' will be the URL to the TangoDevice after the beamline name. Something like 'motors/phi' normally. Currently the connection is not generic, only exposed motors in tango can be connected to - not everything. However arbitrary spec commands can be sent with the 'Spec Command' transformer.

*Attributes*

* Name - Sets the name used in the workflow. Set to something short or it will truncate (the tooltip shows the full name in this case).
* Expression Mode - Options for specifying the flow control of the actor. For instance the actor will run on every message received or block until all messages are received. Each actor can have different options for the values of Expression Mode.
* Motors - Parameter for editing the connections to hardware made when the actor is run. In this case a popup form is shown for adding one or more connections which are always read and optionally written. Upstream variables can be used in substitution notation ( '${var_name}' for instance) in the value and the 'Motor Name' is the last part of the Tango url after the beamline, for instance: 'motors/phi'.

Spec Command
............

This actor sends spec commands into spec. These can either be individual commands, the result of each of which is recorded in a variable and passed on or a single macro file, which does not record output other than in the log file. All commands sent to spec are wrapped in an eval(' ') statement which means they can be typed in to the user interface the same as if entering commands directly into spec. If using the macro file option, you can right click on the actor and open an substitution editor for the maco. This allows upstream variables to be inserted to the macro by highlighting and double clicking the variable on the left. This actor requires tango servers to work and requires the correct tango client settings under 'Window->Prefrences->Data Analysis->Tango'.

*Attributes*

* Name - Sets the name used in the workflow. Set to something short or it will truncate (the tooltip shows the full name in this case).
* Commands - The individual commands to run, the result of each of which may be read back into a separate macro.
* Spec Macro - Reference a spec macro file with this parameter or start a new file. Then right click on the editor and open the editor. Values from up-stream can be substituted in.

Shared Memory Source
....................

This source monitors shared memory and whenever it changes (note it doe nothing when no change occurs), it fires the pipeline with the data read. By default the actor monitors until stopped, i.e. the workflow never stops unless a stop actor is used downstream. There is an expert parameter to specify a timeout if the data does not change for a given time. The monitor can deal with 1D and 2D data being stored in the shared memory. For 1D there is a chunk size to specify the number of 1D arrays to read from the stack of shared memory. Integer and floating point data is supported. The actor uses a Tango client so the correct tango server for reading spec shared memory should be installed.

*Attributes*

* Name - Sets the name used in the workflow. Set to something short or it will truncate (the tooltip shows the full name in this case).
* Chunk Size - The chunk of array data to read from the stack. Each data in the chunk will be forwarded on with a variable named using the 'Output Name' parameter and an 1-based integer for the chunk index. For instance 'shared1' for the first item and 'shared10' for the last when output name = “shared” and chunk size is 10.
* Data type - Either a stack of 1D data or a 2D image.
* Inactive After (expert parameter) - Set to -1 for infinite monitoring or a positive value in ms to define a period beyond which if the data does not change, the workflow will exit.
* Output Name - The variable name which should be used for output data read.
* Source Frequency - The period of checking for new data .If data has changed, it will be read and sent on, if not this actor will wait until another interval of 'Source Frequncy' has passed. Units in ms.
* Spec Name - The name of the spec session to connect to. NOTE you cannot currently connect to arbitrary spec sessions they have to be made available by your beamline contact(s).
* Spec Tango URI - The portion of the URI to identify the spec tango server used, for instance 'spec/shm'.
* Spec Variable - The spec variable in the spec session to monitor. Providing the other attributes are filled in, the system will connect to spec and offer a drop down for this parameter.


