Anne's New Notes

### Procedure/Functions/Custom blocks
So turns out OpenRoberta-Lab DOES let the end-user create their own custom blocks (called functions).
See this file for implementation:
blocks\procedures.js

### Finding duplicates in code
We can either:
1. Check the connected blocks for duplicates (Possibly useful 'getConnections_' here:
core\block.js)
OR
2. Check the generated source code for duplicates
Nr. 2 however, requires us then to generate the source code every time a change is made. Currently, source code is only generated when the user clicks the button to see source code. 

### Creating new block
The folder /core contains general functionalities for blockly blocks. 
The folder /blocks contains the actual blocks. 
I defined my new block (naoActions_move) in this file:
blocks/naoActions.js 

You specify what fields, what text and what inputs the block will have. You also specify which connections the block allows (connection on top of block, and connection on bottom of block). You define the toolTip (text explaining how the block works, which the end-user can see if they want).

After making your block, run this from root folder:
python2.7 build.py

It builds a new blockly_compressed.js and msg-folder. Replace the file and folder in the OpenRoberta-Lab with these.
**HOWEVER! I'm not sure we're gonna add new messages (they seem mostly to be useful for letting the user change language). So perhaps we don't need to copy the msg folder?**
After that, you must re-generate the server and frontend in the OpenRoberta-Lab! 
