###Anne's Notes on Block-Duplication Detection

core/connection.js - Components for creating connections between blocks.
core/connection_db.js - Components for managing connections between blocks.

block.js
childBlocks_ - Array.<blockly.block>
parentBlock_ - one blockly.block
prototypeName - The 'name' of the block. Blockly.Blocks[prototypeName];

getChildren() - get childBlocks_
setParent()
getDescendants()
setParent() - set parent of this block to be new block/null. 
The cur block is removed from the old parent's child list.
Cur block is added to new parent's child list. 

connection.js
setParent(childConnection) is used in connect_()
childConnection is the new child to be attached to 'superior'/parent block.
Disconnect any existing parent on child block.
Is new parent block already connected to something?
    Disconnect child, and try to re-attach it to new child
    If it can't be re-attached, bump out to the side
Update two connections to target each other:
Blockly.Connection.connectReciprocally_(parentConnection, childConnection);

**I think a child block is a nested block??? So not necesariely bottom attached block?? Tjek!!!**

Connection this connection connects to. Null if not connected:
Blockly.Connection.prototype.targetConnection = null;

rendered_connection.js
getDescendants() is used in hideAll() to hide all attached blocks, when a block is collapsed

== Finding start-block ==
Start-block for NAO is defined in blocks/robControls.js
Adding console.log("parent block type: " + parentBlock.type); in connect_(), gets us:
"robControls_start"
Which matches this from robControls.js:
Blockly.Blocks['robControls_start'] = {

== The Graph ==
I have made blocks_graph.js under /core.
Here, I will define a graph, that is updated, whenever connections on the stack are updated.
Each kind of block should be it's own node (so each block is not treated as an individual instance, but as that general type of block.)

The graph is instantiated by the workspace. 
In connection.js, the startBlock will be added to the graph, when you attach a block to it for the first time. 

**Current idea**
Whenever a connection is made in connection.js (on stack), we check whether the graph already contains a connection between the two node-types. 
If it does, we move to the child-block in the connection, and check whether it's connected to a child. If so, we also check if that connection between the types is in the graph already. 
We continue this, untill we hit a break (no more duplicate connections). If the walk is >= 5 nodes long, we've found a duplicate section. 

Fx a program like this: S, 1, 3, 2, 1, 4, 1, 3, 2
We have the section (1,2,3) twice. It first finds 1->3. It checks child-block 3 in the connection, and sees that it has connection 3->2. 2 has no further connections, so it stops. 
We have then found, that the section 1->3->2 is duplicate. 

How to save directed edges in graph? typeName1:typeName2 string maybe?


**blue input block also counts as connection! Fx if program is start->rest->walk(50) then rest->walk has index 1 and (50) has index 2. Attaching another block to walk will give it index 2 as well.**