# ![Pymiere logo](logo.png) Pymiere : Python for Premiere Pro
Use Python to interact with Adobe Premiere Pro. Gather data, check and edit your projects.

## Why using Pymiere?
Pymiere comes from a consideration, has a Pipeline TD in a 3D/VFX studio, that we add no easy/good way to plug Premiere Pro into our workflow.   
Of course, if you want to programmatically create a Premiere file, you can simply use an XML file (see [Open Timeline IO to XML](https://opentimelineio.readthedocs.io/en/latest/tutorials/adapters.html#final-cut-pro-xml)). **But** that require exporting and importing files, potentially loosing some data and no quick visual feedback.     

That's where **pymiere** comes in handy:    
Want to check if some shots have new versions available? Maybe automatically place them on a new track?      
Want to create interactive tools for your editor using Qt, Shotgun API, custom libs...?    

## Installation
  1. Install the _Pymiere Link_ extension for Premiere Pro
      * Download `pymiere_link.zxp` from this repo
      * Install using the [Extension Manager Command Line tool](https://partners.adobe.com/exchangeprogram/creativecloud/support/exman-com-line-tool.html) (note that the `Adobe Extension Manager` UI is deprecated)
        - Download and unzip the folder somewhere
        - Navigate to the folder in Command line or Power shell
        - type `.\ExManCmd.exe /install D:\path_to_extension\pymiere_link.zxp`
      * Alternatively install using [ZXP installer](https://aescripts.com/learn/zxp-installer/) or [Anastasiy Extension Manager](http://install.anastasiy.com)
      * To check that it is correctly installed, start Premiere, under `Window > Extensions` you should see `Pymiere Link` (clicking on it will do nothing)
  
  2. Install the Python lib
      * make sure the `requests` Python lib is installed (`pip install requests`)
      * put the `pymiere` folder somewhere accessible to your Python
      
  3. Try running some basic code:
```python
import pymiere
print(pymiere.objects.app.isDocumentOpen())
```

## Documentation
### Quick start
Open or create a Premiere project containing a sequence with at least a clip. You can now execute the `demo.py` which will demonstrate some basic code. You can also look into the `pymiere/wrappers.py` file to see some more code example.   

Pymiere is at its core a wrapper for _Adobe ExtendScript_ (Adobe flavored javascript for manipulating data in their software).   
Most of the help for ExtendScript will therefore apply to pymiere.    
`pymiere.objects` is your Python entry point to access all Premiere objects and functions. As pymiere offer code completion and type hint in modern IDE, it is easy to navigate/use the objects. Some also have docstrings.    
**Note:** You have to have Premiere Pro running for pymiere to work. If your script needs to know if Premiere Pro is running or start it, some functions are available in `pymiere/exe_utils.py` for that.

### Useful links
* [Official doc for Premiere Pro objects](http://ppro.aenhancers.com/)
* [Unofficial doc for Premiere Pro objects](http://www.brysonmichael.com/premiereapi/objects)
* [Advanced Premiere Pro Extendscript usage](https://github.com/Adobe-CEP/Samples/blob/master/PProPanel/jsx/PPRO/Premiere.jsx)

### Versions
  * support Python 2 & 3   
  * Tested with **Adobe Premiere Pro version 13.0 (2019)** and version 11.0 (2017). I highly recommend the 2019 version because some functionality are not available in the previous versions. It should work for version 2017+ though.
  * Tested on Windows (10)

## Contact
For any support, questions or interest please contact me: <a href="mailto:q.masingarbe@gmail.com">q.masingarbe@gmail.com</a>

## Internal working
Here is how pymiere works:
1. `pymiere` converts the python action (getting a property, executing a function...) to _ExtendScript_ code (ExtendScript is Abode flavored javascript used to access and manipulate programmatically their software)
2. `pymiere` sends the ExtendScript code to the `Pymiere Link` extension via the _requests_ lib (http)
3. `Pymiere Link` _node.js_ server receive the ExtendScript code and execute it within Premiere Pro context
4. If some value is returned `Pymiere Link` will send it back as a _JSON encoded_ response to `pymiere`
5. `pymiere` will then decode the JSON data to have it back in the python context

On top of that, the lib also include a mirror of all Premiere Pro ExtendScript objects in Python so that modern IDE are able to autocomplete and add hint while coding.
These objects mirrors were autogenerated from the Extendscript objects reflection interface.
I did a [detailed article](https://www.linkedin.com/pulse/python-control-adobe-applications-quentin-masingarbe/) about this workflow !

## Futur improvements
 - [ ] separate the generic part handling communication between python and ExtendScript from the specific code for Premiere Pro, enabling its use in other applications (Photoshop, Encoder...)
 - [ ] add more examples & more _wrappers_ functions
 - [ ] add support for Premiere _events_
 - [ ] add more documentation, docstrings...
 - [ ] build one Python mirror of ExtendScript objects by Premiere version, as each version adds new objects/functions/properties
 - [ ] add a way to simply customize a panel to call python functions
 
 ## Thanks
 I'd like to thank everybody that contributed to Pymiere by reporting bugs, sending ideas, ...
 - Isaac brown (https://github.com/ikebenbrown)
 - Roy Nieterau (https://github.com/BigRoy)
