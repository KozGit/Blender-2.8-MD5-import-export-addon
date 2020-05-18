Blender-2.8-MD5-import-export-addon
.MD5 model ( idTech 4 .md5mesh and .md5anim ) import/export addon for Blender 2.8. Supports batch import/export of .md5anim files as actions in Blender.

Based on the io_scene_md5.py script by nemyax.


#MD5 Importer/Exporter for Blender 2.80+

Installation	2
Before you begin: Collections	2
.MD5mesh hierarchy in the Outliner	3
Before you begin: Bone Layers	4
Importing MD5 Meshes	5
Importing MD5 Animations	5
Exporting MD5 Meshes	6
Exporting Individual MD5 Animations	6
Exporting MD5 Mesh and Animation(s)	7

This import/export script provides the following functionality:

Import:
    • Import .md5mesh file
    • Import .individual md5anim file
    • Batch import multiple .md5anim files simultaneously

Export:
    • Export individual .md5mesh file
    • Export individual .md5anim file
        ◦ All frames, or range delimited by Preview Frame Start/Frame Stop 
    • Batch export of .md5mesh, and either the active action or all actions as .md5anim files.

This script script is compatible with Blender 2.80 and later. It was updated to support  Blender 2.8, Collections, Actions, and batch import/export from a script made for use in the  Arx: End of Sun project. The  workflow for  using this script is as follows:

    • Each .MD5 model ( .md5mesh file ) is treated as a unique Collection in Blender.  
    • Multiple Collections can be used to support editing multiple .md5mesh files
    • MD5anim files are imported as actions in Blender, which can be accessed via the Dopesheet.  Batch import/export of actions is supported.
    • Only a subset of the armature’s bones are intended for export.
    • Constraints and drivers can be used freely.
    • The character faces positive Y ( optional )
    • Care is taken to keep object transforms applied.
Installation
Important: If you had a previous version of the script installed, remove it before installation. Use the add-on's Remove button to remove the script, or delete the io_scene_md5 subdirectory manually from your Blender addons directory. For details about the addons directory location, see the Addons page in the Blender wiki.

The script is a single file.

Install this script as an add-on:
1. Open Blender's User Preferences ( Edit→Preferences ) window and go to the Add-ons tab.
2. Click Install from File and specify the downloaded io_scene_md5.zip file or the unpacked io_scene_md5.py file.
3. Enable the ‘Import-Export: id tech 4 MD5 format’ addon. ( Scroll through the list of addons to find ‘Import-Export: id tech 4 MD5 format’.  You can make the search easier by selecting ‘Import-Export’ in the drop down list next to the search field under the ‘Install’ button )

Before you begin: Collections


It is important to understand the concept of Collections as used by this script.  Each .MD5mesh file is imported into a new collection named after the .MD5mesh file.  ( e.g importing ‘imp.md5mesh’ will result in a new collection named ‘imp’) All of the meshes in the MD5 model, and it’s armature, will be imported as individual objects inside this collection.  Each collection can contain multiple mesh objects, but can contain only one armature object.  During .MD5mesh export, all objects inside the collection will be exported.
	
Animations ( .MD5anim files ) can be individually or bulk imported as actions in Blender.  Actions are accessible/selectable  through the Dopesheet/Action editor.  Each armature can have one active action associated with it at a time. The currently active action is visible under the (collection name)_MD5_Armature→Animation section of the Outliner.  

The collection name is important – in order to organize the imported actions, the action name is prepended with ‘ (collection_name)_’.  ( e.g. importing walk1.md5anim in the collection ‘imp’ results in a new action named ‘(imp)_walk1.md5anim’).  

When bulk exporting animations for a collection, the entire database of actions is searched, and all entries beginning with the collection prefix are exported.  If the collection name and the action prefix do not match, the actions will not be exported.  Multiple collections for multiple models with all of their animations can exist at the same time in this manner.   


See the ‘.MD5mesh hierarchy in the Outliner’ diagram further detail
			


.MD5mesh hierarchy in the Outliner 


       
    1. Collection created from importing ‘imp.md5mesh’
    2. Blender mesh object containing a mesh from a .md5mesh file
    • the mesh object name is derived from the shader definition in the .md5mesh file
    3.  Blender armature object containing the armature from the ‘imp.md5mesh’ file
    4.  The currently active action associated with the ‘imp_MD5_Armature’ object
    • The action name is the .md5anim file name prefixed with the collection name. The prefix in the action name must match the collection name for bulk export to work
    5.  Actual Blender mesh (child of blender Mesh Object) imported from .md5mesh file
    6.  Blender material applied to the mesh based on the mesh shader name in the imported file.
    • This material name will be the shader name included in the exported .md5mesh file for this mesh object.  ( Textures can be applied to the material, but only this material name will be exported. )
    7.  Armature modifier applied to the object allowing the MD5_Armature object to deform it
    8.  Blender armature object containing the armature from the ‘hellknight.md5mesh’ file
    9.  The currently active action associated with the ‘hellknight_MD5_Armature’ object
    10.  Blender object containing the skeleton/bones for the armature.
    11.  Hide/Unhide  collection by clicking the eye icon.  Use to control which models to display in the 3d viewport

Before you begin: Bone Layers

For export to work, all bones to be exported must be a member of the specified reserved bone layer in the armature. Bones are automatically added to a selectable reserve layer ( assigned by the ‘Bone Layer’ value in the .md5mesh import dialog) at import.   By default, layer 5 ( for MD5 ) is used.

If the reserve layer has been changed during import, or to select a different layer as a reserve layer to add bones to, go to the ‘Object Data Properties’ for the armature.  Under the ‘MD5 Export Setup, you can define which layer will be check during export.  There are also controls to add/remove/replace/clear bones from the currently defined bone layer.  See image:




     




    1. ‘Object Data Properties’ toolbar icon
    2. MD5 Export Setup Options section
    3. Export Bone Layer.  Only bones found in this layer will be exported.
    4. Bone management controls – add/remove/replace/clear bones in the currently selected bone layer.
Usage
Importing MD5 Meshes
Click File | Import | MD5 Mesh in the main menu.

Options:

    • Reorient Model 
        ◦ This option can be used to re-orient a model to face Blender’s Y axis if desired.  0 Degrees is no rotation, 90 degrees rotates a model facing X to face Y ( 90 deg. Clockwise from above). Depending on your circumstances, you may also want to ensure you correctly re-orient the model on export, and all the associated animations on import and/or export if necessary. 
    • Scale
        ◦ Adjust the scale for all elements in the model
    • Bone layer
        ◦ During import, all bones in the model will be marked as a member of this layer.  During export, all bones are checked to see if they are members of the layer defined in the ‘Object Data Properties→MD5 Mesh Export setup’ section of the properties toolbar for the armature.
          Ensure the bone layers match, or they will NOT be exported.
Importing MD5 Animations

    1. Select an object (e.g. armature) in the collection you want to add animation to. The armature in the collection needs to match the skeleton in the file you are going to import.
    2. Click File | Import | MD5 Animation in the main menu.
    3. Select one or more .md5anim files to import.  Animations are imported as actions which can be access through the Dopesheet Action editor. 
        ◦ Each armature can have one active action associated with it at a time. The currently active action is visible under the ( collection name )_MD5_Armature→Animation section of the Outliner.  
        ◦ The collection name is important – in order to organize the imported actions, the action name is prepended with ‘ (collection_name)_’.  ( e.g. importing walk1.md5anim into the collection ‘imp’ results in a new action named ‘(imp)_walk1.md5anim’).  
Options:

    • Reorient Model 
        ◦ This option can be used to re-orient a the animations to face Blender’s Y axis if desired.  0 Degrees is no rotation, 90 degrees rotates a model facing X to face Y ( 90 deg. Clockwise from above). Depending on your circumstances, you may also want to ensure you correctly re-orient the model on export, and all the associated animations on import and/or export if necessary. 
    • Scale
        ◦ Adjust the scale for all elements in the animation



If everything is OK, the animations will be imported as actions.  If there are errors during import/batch import, please check the Console Window for additional detail.  Batch import does not abort on failure – it will attempt to import all selected .md5anim files and provide a status report when finished.

Exporting MD5 Meshes
    1. Select an object in the collection that you want to export. The meshes should be associated with the same deforming armature. Any selected meshes that do not have an armature modifier will be ignored.
    2. Click File | Export | MD5 Mesh in the main menu.

Options:

    • Reorient Model 
        ◦ Unlike idTech4, Blender assumes the ‘forward’ direction for character rigs to be positive Y. This option can be used to re-orient the mesh to face +X ( idTech ) instead of  +Y axis if desired.  -90 Degrees rotates from +Y to +X. 0 Degrees is no rotation, 90 degrees rotates from +X to +Y. 
    • Scale
        ◦ Adjust the scale for all elements in the animation
                                  

 Specify the file path and complete the export.

Exporting Individual MD5 Animations

    1. Select an object in the collection containing the animated object you are exporting an animation for.
    2. Ensure the desired action is currently assigned to the armature in the collection. ( See item 4 in the ‘MD5mesh hierarchy in the Outliner’ diagram
    3. By default,. all frames of the action are exported by default.  If you would like to export a range of frames instead, set the ‘Start’ and ‘End’ frame fields in the timeline/dopesheet to the desired frames. (
    4. Click File | Export | MD5 Animation in the main menu.

Options:

    • Reorient Model 
        ◦ Unlike idTech4, Blender assumes the ‘forward’ direction for character rigs to be positive Y. This option can be used to re-orient the animation to face +X ( idTech ) instead of  +Y axis if desired.  If you re-oriented the model on export, you will most likely need to re-orient the animation as well. -90 Degrees rotates from +Y to +X. 0 Degrees is no rotation, 90 degrees rotates from +X to +Y.  

    • Scale
        ◦ Adjust the scale for all elements in the animation
    • Use timeline Start/End Frames
        ◦ Select this to only export the frames indicated by the timeline/dopesheet ;Start’ and ‘End’ frame fields.

Specify the file path and complete the export.



Exporting MD5 Mesh and Animation(s)

    1. Select an object in the collection containing the object and actions you would like to export.\
    2. If exporting only a single action, ensure the desired action is currently assigned to the armature in the collection. ( See item 4 in the ‘MD5mesh hierarchy in the Outliner’ diagram
    3. If exporting multiple actions, remember the collection name is important. In order to organize the list of actions, the action names are prepended with ‘ (collection_name)_’.  ( e.g. if importing walk1.md5anim in the collection ‘imp’, a new action named ‘(imp)_walk1.md5anim’ is created). When bulk exporting animations from a collection, the entire database of actions is searched, and all entries beginning with the collection prefix are exported.  If the collection name and the action prefix do not match, the actions will not be exported.  
    4. Click File | Export | MD5 Mesh and Animation(s) in the main menu.
       

Options:

    • Export All Anims
        ◦ By default, ‘Export MD5 Mesh and Animation(s) only exports the .md5mesh file generated from the collection, and the .md5anim file generated from the action currently assigned to the armature of the collection.  Enabling this option will export ALL actions/animations for the collection. The collection name is important - in order to organize the list of actions, the action names are prepended with ‘ (collection_name)_’.  ( e.g. if importing walk1.md5anim in the collection ‘imp’, a new action named ‘(imp)_walk1.md5anim’ is created). When bulk exporting animations from a collection, the entire database of actions is searched, and all entries beginning with the collection prefix are exported.  If the action name does not start with the correct collection prefix, it will not be exported.  
        ◦ All frames in the actions will be exported.
    • Strip action name prepend
        ◦ If enabled, collection name prefixes are removed from the exported file names. (e.g. action ‘(imp)_walk1.md5anim’ exports to ‘walk1.md5admin’
    • Use timeline Start/End Frames
        ◦ Select this to only export the frames indicated by the timeline/dopesheet ;Start’ and ‘End’ frame fields. Has no effect if ‘Export All Anims’ is selected.
    • Reorient Model 
        ◦ Unlike idTech4, Blender assumes the ‘forward’ direction for character rigs to be positive Y. This option can be used to re-orient the animation to face +X ( idTech ) instead of  +Y axis if desired.  If you re-oriented the model on export, you will most likely need to re-orient the animation as well. -90 Degrees rotates from +Y to +X. 0 Degrees is no rotation, 90 degrees rotates from +X to +Y.  
    • Scale
        ◦ Adjust the scale for all elements in the animation
      

Specify the file path for the .md5mesh and complete the export.  The animation(s) will be exported into the same directory as the .md5mesh file.
          

Handling Shaders
When exporting .md5mesh files, the add-on assigns the shader for a mesh in the .md5mesh file the name of the material assigned to that mesh in Blender. The composition of the material itself doesn’t matter, only the name of the material is used.

During export, the exact material name from the first non-empty material slot for the mesh is used as the shader name. If there are no materials on a mesh, the name “default” is exported.

During import, a material is added to the new blender mesh’s first material slot with the exact name of the shader in the .md5mesh file.

Export Errors
The script can detect a few situations where MD5 export is not possible, and pops up an error message instead of the file selector if any of them occurs. If you launched Blender from a terminal, the error message is duplicated there. More detail is also available in the Console Window.
The following problems are reported:

• No armature-deformable meshes in the selection
• The selected meshes are deformed by more than one armature
• No deforming armature is associated with the selection
• The deforming armature has no bones in the reserved layer
• One or more exportable bones have parents outside the reserved layer
• Multiple root bones
• Vertices without deformation weights
• Vertices with deformation weights set to zero
• No UV coordinates in a mesh

The last three checks are not done for individual .md5anim export. This lets you use meshes that have these characteristics (for example, if you want custom bounds in your animation).
For problem-free export, address the above issues in advance.






