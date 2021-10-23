---
layout: page
title: Blender Viewport Navigation
---
[Back to Blender Notes Index](/resources/notes/blender/)

These notes are based on the [Viewport Navigation][1] video tutorial.

[1]: https://studio.blender.org/training/blender-2-8-fundamentals/interface-overview/

## Status Bar

At the very bottom of the window there is a [Status Bar] that indicates the
actions that certain mouse clicks will perform, in the context of which hot keys
you are pressing, and which objects your mouse cursor is pointing at in the
window.

![status bar][status bar image]

You can toggle the status bar from the 'Window' menu using the 'Show Status Bar'
option.

[Status Bar]: https://docs.blender.org/manual/en/latest/interface/window_system/status_bar.html
[status bar image]: /images/resources/notes/blender/interface-overview/status-bar.gif

## Default Workspaces

Blender provides default [Workspaces] for common workflows. These are:

* __Layout__ - general workspace to preview your scene and objects
* __Modeling__ - For modification of geometry by modeling tools
* __Sculpting__ - For modification of meshes by sculpting tools
* __UV Editing__ - Mapping of image texture coordinates to 3D surfaces
* __Texture Paint__ - Tools for coloring image textures in the 3D Viewport
* __Shading__ - Tools for specifying material properties for rendering
* __Animation__ - Tools for making properties of objects dependent on time
* __Rendering__ - For viewing and analyzing rendering results
* __Compositing__ - Combining and post-processing of images and rendering
  information
* __Geometry Nodes__ - For procedural modeling using Geometry Nodes
* __Scripting__ - Programming workspace for writing scripts

![workspaces][workspaces image]

After you've made modifications to the panels in a workspace, those
modifications remain even if you switch between workspaces.

You can use CTRL + Page Up or CTRL + Page Down to switch between workspaces.

[Workspaces]: https://docs.blender.org/manual/en/latest/interface/window_system/workspaces.html
[workspaces image]: /images/resources/notes/blender/interface-overview/workspaces.gif

## Resizing Panels

You can place your mouse cursor on the boundary of a panel, and you will see
that the cursor icon turns into an adjustment icon. Click and drag to adjust the
size of the panel.

![panel adjust][panel adjust]

[panel adjust]: /images/resources/notes/blender/interface-overview/panel-adjust.gif

### Toggle Maximized

You can maximize certain panels by placing your mouse cursor over them, and
then clicking on CTRL + Spacebar to toggle between a maximized and default
sized panel.

### Change Editor Type

You can click on the 'Editor Type' icon in the upper-left corner of a panel
to change the type of editor that is showing in the panel.

![change editor type][editor type]

There are many types of editors you can choose from.

![editor type options][editor type options]

[editor type]: /images/resources/notes/blender/interface-overview/editor-type.gif
[editor type options]: /images/resources/notes/blender/interface-overview/editor-type-options.gif

### Splitting Areas

If you right-click on the boundary of a panel, you can choose 'Swap Areas' to
swap the two panels that share the same boundary.

You can also choose to split a panel into two panels horizontally or vertically.
The split doesn't have to be related to the boundary of the panels you were
right-clicking on, you can move your mouse cursor and choose to split
any of the panels.

For instance, if you choose 'Vertical Split', then a vertical
line will display over any of the panels your mouse cursor points over. As soon
as you click inside of the panel, the split will take place upon the line you
placed.

### Removing Panels

If you wish to remove a panel, right-click on the boundary of two panels and
select 'Join Areas'. You'll then be given the option to point at one of the
panels that you want to remove, then left-click to remove it, thus leaving the
other panel present.

![join areas][join areas]

[join areas]: /images/resources/notes/blender/interface-overview/join-areas.gif

## 3D Viewport

### Toggle Toolbar

You can use the 't' key to toggle the display of the Toolbar.

![Toolbar][toolbar image]

You can also use SHIFT + Spacebar to view the Popup Toolbar.

![Toolbar Hotkey menu][toolbar hotkey menu image]

[toolbar image]: /images/resources/notes/blender/interface-overview/toolbar.gif
[toolbar hotkey menu image]: /images/resources/notes/blender/interface-overview/toolbar-hotkey.gif

For more information, see [Tool System].

[Tool System]: https://docs.blender.org/manual/en/latest/interface/tool_system.html

### Toggle Sidebar

There is a [3D Viewport Sidebar] that can be toggled using the 'n' key.
When it's hidden you can also display it by clicking on the little arrow
shown on the top right corner of the 3D Viewport panel.

![Sidebar hidden][sidebar arrow image]

[sidebar arrow image]: /images/resources/notes/blender/interface-overview/sidebar-arrow.gif
[3D Viewport Sidebar]: https://docs.blender.org/manual/en/latest/editors/3dview/sidebar.html

#### Sidebar Item Transform

The 'Item' tab provides a Transform area that displays various properties of th
object that is currently selected, and allows you to click and drag on the
values for those settings to adjust them.

![Sidebar Item Transform][sidebar item image]

[sidebar item image]: /images/resources/notes/blender/interface-overview/sidebar-item.gif

#### Sidebar Tool

The 'tool' tab allows you to modify various settings related to the currently
selected tool in the Toolbar.

![Sidebar Tool Active Tool][sidebar tool image]

[sidebar tool image]: /images/resources/notes/blender/interface-overview/sidebar-tool.gif

#### View

The 'view' tab provides you with other 3D Viewport options, such as the settings
for the 3D Cursor.

![Sidebar View][sidebar view image]

The 3D Cursor is used to determine where new 3D objects should be placed
(spawned), and also acts as a reference for pivoting (used in animations).

You can use SHIFT + Right-click to place the 3D Cursor inside of the 3D viewport
area. You can also press SHIFT + S to view a menu of options that lets you
place the 3D Cursor in commonly desired locations, such as the 'World Origin'
which is the center of the entire scene.

[sidebar view image]: /images/resources/notes/blender/interface-overview/sidebar-view.gif

#### Pie Menus

There are various [3D Viewport Pie Menus] that display around your mouse cursor
when certain Hotkey combinations are pressed. The previous SHIFT + S example
for placing the 3D cursor is an example of this. They call it a "Pie" menu
because the options display around a circle, with each option being like a
slice of the pie.

![3D Cursor Pie Menu][3D Cursor Pie Menu image]

A pie menu will display after certain hot-keys are pressed, and will remain
on the screen until you move your mouse and left-click to select the highlighted
option. If you change your mind, you can right-click to cancel.

Optionally you can press and hold the hotkeys, move your mouse, and then release
the hotkeys with the option you want selected to be executed.

[3D Viewport Pie Menus]: https://docs.blender.org/manual/en/latest/addons/interface/viewport_pies.html
[3D Cursor Pie Menu image]: /images/resources/notes/blender/interface-overview/3d-cursor-pie-menu.gif

## Timeline

Below the 3D Viewport there is a [Timeline] panel. You may need to drag the
boundary of this to make it larger, as it is minimized by default.

The Timeline is used to control playback of animations. You can use your mouse
scroll wheel to "zoom in" and "zoom out" of the timeline. You can also
drag the timeline with your middle mouse button.

![Timeline panel][timeline panel image]

[Timeline]: https://docs.blender.org/manual/en/latest/editors/timeline.html
[timeline panel image]: /images/resources/notes/blender/interface-overview/timeline.gif

## Properties

The [Properties] panel contains several tabs shown vertically with different
icons, and is used to display the properties of your scene.

* _Tool Properties_
  * __Active Tool and Workspace Settings__ - Options for your currently selected
  tool in the Toolbar, similar to the 'Tool' tab in the Sidebar.
* _Scene Properties_
  * __Render Properties__
  * __Output Properties__
  * __View Layer Properties__
  * __Scene Properties__
  * __World Properties__
  * __Object Properties__
  * __Physics Properties__
  * __Object Constraint Properties__
  * __Object Data Properties__
  * __Texture Properties__

[properties]: https://docs.blender.org/manual/en/latest/editors/properties_editor.html

## Scenes

You can create more than one [scene] in a single Blender file.

[scene]: https://docs.blender.org/manual/en/latest/scene_layout/scene/index.html
