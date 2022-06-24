![](./DemoExamples/EasyAimPrimAlignAIM.gif)

# Technical Art Demos

## SKEW LINES SOLVER
CPA I IMPLEMENTED
https://core.ac.uk/download/pdf/74237799.pdf
Photoshop
Maya
Max
Fusion360 was a 80K long single line crash editor lol
Moi3d
Modo
Blender
Zbrush
Substance Designer
Substance Painter

## A Generic Hotkey Configuration Optimizer
Hotkey framework for program reconfiguration

Optimize UI the ui layout
	Healthy artists (ergonomics)
	Faster artists (more efficient)
	Ulnar deviation
	Allowing the hand to rest on the homerow
	More creative artist -> easier to get in the flow
	the comptuer gets out of the way

Document the layout for east artist reference

Extend the assignable configuration keys via autohotkey
Maintainable
Have a repository per user where they can store it remotely and back it up as
well as view changes and make changes if needed


Photoshop example
Look at photoshops configuration file
reverse engineer the data storage format and ordering
derive 3 main command types
	static unique id
	dynamic no unique id
	tools stackable groups vs non stackable
Flood the UI with all possible keycombinations
build a map between UI representation of a command and its internal xml storage representation
determine a sort order for left vs right handed artists
log artist sessions in photoshop and sort most used vs least used commands according to frequency
reconfigure the ui to be most accessable to the artist
On top of that override photoshops reserved keys for things like brush size and layer manipulation
using autohotkey
Parsing photoshop style input sequences to autohotkey syntax hotkey strings
	output an authotkey file with these recursive maps

when I hit alt 1 I want to decrese brush size
again keeping the data self documenting I automatically generate a comment string as well

; Decrease Brush Size
!1::
{
    SendInput, {[}
    return
}

Finally combining both the recursive autohotkey maps as well as the photoshop
mappings I parse those sequence strings to be readable in html visualizer,
which is a simple webpage whose html slots (html hotkey sequence as
key)at each keyboard location take in a text string
(value)
 ------------------------------------------------------------------------------
## MAYA PROJECTS BREAKDOWN
Unchamfer ->
Using mayas lower level python api, which really is a procedurally generated
python api that has a 1 to 1 binding to its c++ api via SWIG
simplified wrapper and interface generator
https://www.swig.org/

Vector Line Intersection:
https://vicrucann.github.io/tutorials/3d-geometry-algorithms/#skew-lines-geometry-shortest-distance-and-projection-of-one-skew-line-onto-another
http://paulbourke.net/geometry/pointlineplane/

CPA algorithim:
https://core.ac.uk/download/pdf/74237799.pdf

Graphics Gems Volume 2
Page 326 sample code:
http://www.realtimerendering.com/resources/GraphicsGems/gemsii/xlines.c

Alternate case:
non perfect intersections = CPA solver take the minimum on each vector then average

EdgeCase:
corner bevels of a 3 way intersection (triangle corner) Cpa Solving
This required recursive bevel operations, performing the solver twice


The first challenge apart from building a math library that did not exist in
maya (extending the API) was
mesh traversal
how to traverse along an edge loop or ring
how to identify branch points (triangles)
how to organize and store the data in such a way that I know the directions
of each vector, what to input where, and keep tract of what to finall move
to a resulting location

at triangles
	how to do recursive 3 way solvers between A B C vectors

(Something I think would be useful in our javascript performance issue)
Performance issues, cmds wasnt cutting it and api forces you to compact your
data into arrays and reduce the api calls by tightly organizing your data
calling eveyrthing you need at once and sorting it yourself.

CmdsSnapalign
Built a snapping system from the ground up
Calculates the face center of a polygon even if a real vertex does not exist
easy align objects
all the math same challenges data and math
select neraest face center
edge center
or vertex
purely based on cursor position no clicking
then snap and align
predictive
always Y UP

Smart Tool Handle Activation: (Lazy Boi Mode)
Based on Manipulator to Camera Space projection, SnapAlign can calculate the 
nearest active tool handle to the cursor, it does this by first projecting
the gizmo axis onto the camera plane, then does a 2D vector angle calculation
between the projected gizmo axis angles (-X -Y -Z +X +Y +Z) and the cursor
to projected gizmo Vector, by finding the minimum delta vector, we can dervice
the "closest" active tool handle to cursor.
See the demo here:

This can easily be extended to multiple tools, including duplicate special,
wherein the active axis for the duplicate operation is determined by the cursor
position! No more menus!

SnapAlign in action, with Smart Tool Handle Activation turned ON:


PRIMALIGN
Aligns objects to the component (edge center face center OR vertex normal)
under the cursor. If the object is a newly created primitive at world center
PrimAlign will move and bake the selected Object's Object Space -Y bounding box
to the the grid floor prior to alignment transform. This allows for predictable
alignment of the object's ground plane to the target geometry's alignment
plane.

Secondary Function, when an active axis of the move tool is selected and a component
is selected, PrimAlign will aim the active axis towards the component center.
Either edge center or face center for edges and faces, or vertex if a vertex is selected.


RECORD WITH PLANAR DETECTION ON
PLANEFLATTEN
Consecutive Faces selected will align the first face to the second face ALONG
the connected edges as vector Angles

Seperated Face selections will porject along the vectors of each face, results
in a skew like effect per face towards the target plane alignment

Select 3 vertex of a non planar face and planar align that face while respecting
surround geometry






















