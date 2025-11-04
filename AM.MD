### Anne's Notes

### Creating new block
I defined a new 'Action'-block here:
blocks\naoActions.js

Then run this from root folder:
python2.7 build.py

It builds a new blockly_compressed.js and msg-folder. Replace the file and folder in the OpenRoberta-Lab with these.
**Do we need to regenerate the server if these are changed? Or is it only relevant for when we change the other files in OpenRoberta-Lab?**

### Procedure/Functions/Custom blocks
So turns out OpenRoberta-Lab DOES let the end-user create their own custom blocks (called functions).
See this file:
blocks\procedures.js

Possibly useful 'getConnections_' here:
core\block.js

### Finding duplicates in code
So we can either:
1. Check the connected blocks for duplicates
OR
2. Check the generated source code for duplicates
Nr. 2 however, requires us then to generate the source code every time a change is made. Currently, source code is only generated when the user clicks the button to see source code. 


### Possible Problem
The front-end Blockly buttons can only be generated using this code. Since it is separated from the OpenRoberta-Lab code, that may cause problems when creating new custom block?
Perhaps we could create a placeholder. So we create the 'define'-block once, and then use a placeholder for the new block. 
The user should be able to set the name of the new block. Roberta does have a field for text-input - Could be used? But should name be mutable?