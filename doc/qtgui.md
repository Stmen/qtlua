<a name="qtgui.dok"></a>
# QtGui Bindings #

The package `qtgui` 
contains bindings for classes defined by the Qt module
[QtGui](http://doc.trolltech.com/4.4/qtcore.html).

Besides the capabilites reported below, all qt variants inherit a set
of [default methods](qt.md#qt.QVariants), and all qt object
classes inherit the capabilities from their superclasses and
automatically expose [[qt.md#qt.QObjects|properties, slots and
signals]].


<a name="qaction"></a>
## qt.QAction, qt.QtLuaAction ##
<a name="qtluaaction"></a>

Qt class 
[QAction](http://doc.trolltech.com/4.4/qaction.html) 
is a sublass of [qt.QObject](qtcore.md#qt.qobject)
that represents actions associated with menu items
and toolbar icons.  


<a name="qaction"></a>
### qt.QAction([arg]) ###

Expression `qt.QAction([arg])` returns a new `QAction` object.
The optional argument `arg` can be a qt object that
becomes the action parent or a table whose
members contain initial values for the action properties.
This table can also contain a function value 
which is then connected to the signal `triggered(bool)`
of the qt action object.

Example:
```lua
a === qt.QAction { text="Automatic Mode", 
                   checkable === true, checked === false,
                   function(b) setAutoMode(b) end }
```

Unless a parent is specified, the action is owned
by the Lua interpreter and therefore is automatically 
destroyed when the garbage collector determines that 
it is no longer referenced.

Actions are always created from the main thread using
the [thread hopping](qt.md#qt.qcall) mechanism.

<a name="qaction.menu"></a>
### qaction:menu() ###

Same as the C++ function `QAction::menu()`.

<a name="qaction.setmenu"></a>
### qaction:setMenu() ###


Same as the C++ function `QAction::setMenu()`.

<a name="qtluaaction"></a>
### qt.QtLuaAction([arg]) ###

This constructor takes the same arguments as `qt.QAction`
but returns an instance of a convenient subclass
that enables or disables the action according
to the availability of the Lua engine.

The action follows its property `enabled`
when the Lua engine is ready to invoke Lua 
functions connected to Qt signals.
Otherwise the action is disabled regardless
of the value of property `enabled`.

<a name="qtluaaction.autodisable"></a>
### qtluaaction.autoDisable ###

This property applies only to action objects of class `qt.QtLuaAction`.
It controls whether the action is automatically disabled
when the Lua engine is not available. 
The default is `true`.


<a name="qapplication"></a>
## qt.QApplication ##

Qt class [QApplication](http://doc.trolltech.com/4.4/qapplication.html)
manages the GUI application's control flow and main settings. This is a 
subclass of [QCoreApplication](qtcore.md#qcoreapplication).

<a name="qapplication.changeOverrideCursor"></a>
### qt.QApplication.changeOverrideCursor(qcursor) ###

Changes the currently active application override cursor to cursor
without disturbing the cursor stack. This function has no 
effect if `setOverrideCursor()` was not called.

<a name="qapplication.keyboardModifiers"></a>
### qt.QApplication.keyboardModifiers() ###

Returns a string describing the currently depressed keyboard modifiers.

<a name="qapplication.mouseButtons"></a>
### qt.QApplication.mouseButtons() ###

Returns a string describing the currently depressed mouse buttons.

<a name="qapplication.overrideCursor"></a>
### qt.QApplication.overrideCursor() ###

Returns the active application override cursor.
This function returns `nil` if no application cursor 
has been defined (i.e. the internal cursor stack is empty).

<a name="qapplication.restoreOverrideCursor"></a>
### qt.QApplication.restoreOverrideCursor() ###

Undoes the last `setOverrideCursor()`.

<a name="qapplication.setoverridecursor"></a>
### qt.QApplication.setOverrideCursor(qcursor) ###

Sets the application override cursor to `qcursor`.
Application override cursors are intended for showing the user that 
the application is in a special state, for example during an operation that might take some time.
Override cursors are stored on a stack. Function `setOverrideCursor()` pushes a cursor on the stack.
Function `restoreOverrideCursor()` pops the stack.


<a name="qbrush"></a>
## qt.QBrush ##

Qt class 
[QBrush](http://doc.trolltech.com/4.4/qbrush.html) 
represents the painter settings that determine how shape
are filled and how text is rendered.  Brushes are represented
by Qt variants of class `qt.QBrush`.


<a name="qbrush"></a>
### qt.QBrush(table) ###
<a name="qbrushfromtable"></a>

Expression `qt.QBrush(table)` returns a brush whose settings
are determined by the fields of table `table`. 
The following fields are recognized:

  * Field `color` contains a [qt.QColor](#qcolor) value representing the brush color. 
  * Field `texture` contains a [qt.QImage](#qimage) value representing the brush texture.
  * Field `gradient` contains a [qt.QGradient](#qgradient) value representing a color gradient.
  * Field `transform` contains a `qt.QTransform` value that defines a additional transformation applied to the brush before painting it into the current coordinate system.
  * Field `style` contains the name of the [brush style](http://doc.trolltech.com/4.4/qt.html#BrushStyle-enum). The default brush style is `SolidPatter` when field `color` is defined, `TexturePattern` when field `texture` is defined, or one of the gradient styles when field `gradient` is defined.  Otherwise the default style is `NoBrush` meaning that nothing is painted.


<a name="qbrush.totable"></a>
### qbrush:totable() ###

Expression `qbrush:totable()` returns a table describing the brush.
See the documentation of [qt.QBrush(table)](#qbrushfromtable)
for a description of the table fields.


<a name="qcolor"></a>
## qt.QColor ##

Qt class 
[QColor](http://doc.trolltech.com/4.4/qcolor.html) 
represents colors for painting purposes.
Qt variant of class `qt.QColor` have a textual representation
of the form =="#RRGGBB" where letters `R`, `G`, or `B` are
hexadecimal digits representing the intensities of the 
color components.


<a name="qcolor"></a>
### qt.QColor(...) ###

There are several ways to construct a Qt variant of class `qt.QColor`.

__`qt.QColor(r,g,b,[a])`__

Arguments `r`, `g`, `b`, and `a` are numbers in range `[0,1]`
representing the intensities of the red, green, blue, and alpha channels.
The default value for argument `a` is `1` for a fully opaque color.

__ `qt.QColor(string)` __

Argument `string` is a string representing a color name.
All [SVG color names](http://www.w3.org/TR/SVG/types.html#ColorKeywords)
are recognized.Color names can have also the format `"#RGB"`, 
`"#RRGGBB"`, `"#RRRGGGBBB"`, or =="#RRRRGGGGBBBB"
where letters `R`, `G`, or `B` represent hexadecimal 
digits for each of the color component.

__ `qt.QColor(table)` __

Argument `table` is a table whose fields `r`, `g`, `b` and `a`
contain numbers in range `[0,1]` representing the intensities
of the red, green, blue, and alpha channels.  Such a table 
is returned by function [qcolor:totable()](#qcolortotable).

<a name="qcolor.totable"></a>
### qcolor:totable() ###
<a name="qcolortotable"></a>

Expression `qcolor:totable()` returns a table whose fields
`r`, `g`, `b` and `a` contain numbers in range `[0,1]` 
representing the intensities of the red, green, blue, 
and alpha channels.


<a name="qcolordialog"></a>
## qt.QColorDialog ##

Qt class
[QColorDialog](http://doc.trolltech.com/4.4/qcolordialog.html) 
implements a dialog for selecting colors.  
The recommended way to use this class is the static function `getColor`.

<a name="qcolordialog.getcolor"></a>
### qt.QColorDialog.getColor([font],[parent]) ###

Pops up a dialog for selecting a color.
Optional argument `color` is the initial color selection.
Argument `parent` is the parent widget.
This function returns `nil` if the no color was selected.
Otherwise it returns a qvariant of type [qt.QColor](#qcolor).

<a name="qcursor"></a>
## qt.QCursor ##

Qt class 
[QCursor](http://doc.trolltech.com/4.4/qcursor.html) 
provides a mouse cursor with an arbitrary shape.

<a name="qcursor"></a>
### qt.QCursor(cursorshape) ###

Returns a standard cursor.
Argument `cursorshape` is the name of a 
[standard shape](http://doc.trolltech.com/4.4/qt.html#CursorShape-enum).

<a name="qcursor"></a>
### qt.QCursor(image,[mask],[posx,[posy]) ###

Constructs a cursor with the specified image, mask and hot spot.

<a name="qcursor.hotSpot"></a>
### qcursor:hotSpot() ###

Return a `qt.QPoint` with the cursor's hot spot coordinates.

<a name="qcursor.mask"></a>
### qcursor:mask() ###

Return the cursor's mask.

<a name="qcursor.pixmap"></a>
### qcursor:pixmap() ###

Return the cursor's pixmap.

<a name="qcursor.shape"></a>
### qcursor:shape() ###

Return the name of the cursor's
[standard shape](http://doc.trolltech.com/4.4/qt.html#CursorShape-enum).

<a name="qcursor.pos"></a>
### qt.QCursor.pos() ###

Returns a `QPoint` with the global coordinates of the mouse cursor.

<a name="qcursor.setPos"></a>
### qt.QCursor.setPos(qpoint) ###

Sets the global coordinates of the mouse cursor.

<a name="qdialog"></a>
## qt.QDialog ##

Qt class 
[QDialog](http://doc.trolltech.com/4.4/qdialog.html) 
is the base class of dialog windows.

<a name="qdialog"></a>
### qt.QDialog([parent]) ###

Expression `qt.QDialog(parent)` returns a new `QDialog` instance.
The optional argument `parent` specifies the widget parent.
When argument `parent` is `nil` or not specified,
the new widget is owned by the Lua interpreter 
and is automatically destroyed when the garbage collector
determines that it is no longer referenced.

<a name="qdialog.result"></a>
### qdialog:result() ###

Gets the dialog's result code as an integer.
This is usually `1` when the dialog has been accepted
and `0` when the dialog has been rejected.

<a name="qdialog.setresult"></a>
### qdialog:setResult(integer) ###

Sets the dialog's result code.
This is usually `1` when the dialog has been accepted
and `0` when the dialog has been rejected.


<a name="qfiledialog"></a>
## qt.QFileDialog ##

Qt class 
[QFileDialog](http://doc.trolltech.com/4.4/qfiledialog.html) 
provides a dialog that allow users to select files or directories. 
This is a subclass of [qt.QDialog](#qdialog).

<a name="qfiledialog.getExistingDirectory"></a>
### qt.QFileDialog.getExistingDirectory([p.[c,[d,[opt]]]]) ###

This convenience function shows a file dialog
for selecting an existing directory.

  * Argument `p` is the parent widget. 
  * Argument `c` is the dialog caption.
  * Argument `d` is the initial directory shown in the dialog.
  * Argument `opt` are the [file dialog options](http://doc.trolltech.com/4.4/qfiledialog.html#Option-enum).

The function returns a `qt.QString` containing the selected directory name.

<a name="qfiledialog.getOpenFileName"></a>
### qt.QFileDialog.getOpenFileName([p,[c,[d,[f,[s,[opt]]]]]]) ###

This convenience function shows a file dialog
for selecting an existing file.

  * Argument `p` is the parent widget. 
  * Argument `c` is the dialog caption.
  * Argument `d` is the initial directory shown in the dialog.
  * Argument `f` contains the filters separated by double semicolons.
  * Argument `s` contains the selected filter
  * Argument `opt` are the [file dialog options](http://doc.trolltech.com/4.4/qfiledialog.html#Option-enum).

The function returns a `qt.QString` containing the selected file name
and a `qt.QString` containing the selected filter.

<a name="qfiledialog.getOpenFileNames"></a>
### qt.QFileDialog.getOpenFileNames([p,[c,[d,[f,[s,[opt]]]]]]) ###

This convenience function shows a file dialog
for selecting multiple existing files.

  * Argument `p` is the parent widget. 
  * Argument `c` is the dialog caption.
  * Argument `d` is the initial directory shown in the dialog.
  * Argument `f` contains the filters separated by double semicolons.
  * Argument `s` contains the selected filter
  * Argument `opt` are the [file dialog options](http://doc.trolltech.com/4.4/qfiledialog.html#Option-enum).

The function returns a `qt.QStringList` containing the file names
and a `qt.QString` contaning the selected filter.

<a name="qfiledialog.getsavefilename"></a>
### qt.QFileDialog.getSaveFileName([p,[c,[d,[f,[s,[opt]]]]]]) ###

This convenience function shows a file dialog
for selecting a file name for saving data.

  * Argument `p` is the parent widget. 
  * Argument `c` is the dialog caption.
  * Argument `d` is the initial directory shown in the dialog.
  * Argument `f` contains the filters separated by double semicolons.
  * Argument `s` contains the selected filter
  * Argument `opt` are the [file dialog options](http://doc.trolltech.com/4.4/qfiledialog.html#Option-enum).

The function returns a `qt.QString` containing the selected file name
and a `qt.QString` contaning the selected filter.

<a name="qfiledialog.new"></a>
### qt.QFileDialog.new([parent.[caption,[dir,[filters]]]]) ###

Function `qt.QFileDialog.new` returns a new file dialog object.

  * Argument `parent` is the parent widget
  * Argument `caption` is the dialog caption.
  * Argument `dir` is the initial directory shown in the dialog.
  * Argument `filters` contains the filters separated by double semicolons.

When argument `parent` is `nil` or not specified,
the new widget is owned by the Lua interpreter 
and is automatically destroyed when the garbage collector
determines that it is no longer referenced.

<a name="qfiledialog.directory"></a>
### qfiledialog:directory() ###

Returns a `qt.QString` containing the path of the selected directory.

<a name="qfiledialog.nameFilters"></a>
### qfiledialog:nameFilters() ###

Returns a `qt.QStringList` containing the name filters.

<a name="qfiledialog.selectedFiles"></a>
### qfiledialog:selectedFiles() ###

Returns a `qt.QStringList` containing the selected files.

<a name="qfiledialog.selectFile"></a>
### qfiledialog:selectFile(fname) ###

Select file `fname` in the file dialog.

<a name="qfiledialog.selectNameFilter"></a>
### qfiledialog:selectNameFilter(filter) ###

Select name filter `filter` in the file dialog.

<a name="qfiledialog.setDirectory"></a>
### qfiledialog:setDirectory(dirname) ###

Set the file dialog directory to `dirname`.

<a name="qfiledialog.setNameFilters"></a>
### qfiledialog:setNameFilters(filters) ###

Set the file dialog name filters to `filters`.
Argument `filters` may be a `qt.QStringList`
or a string containing the filters separated by 
two semicolons `";;"`.

<a name="qfiledialog.setOption"></a>
### qfiledialog:setOption(option, bool) ###

Sets the specified
[file dialog option](http://doc.trolltech.com/4.5/qfiledialog.html#Option-enum)
to the boolean value `bool`.  Not available before Qt-4.5.

### qfiledialog:testOption(option) ###

Returns the value of the specified
[file dialog option](http://doc.trolltech.com/4.5/qfiledialog.html#Option-enum).
Not available before Qt-4.5.

<a name="qfont"></a>
## qt.QFont ##

Qt class 
[QFont](http://doc.trolltech.com/4.4/qfont.html) 
represents the painter settings that determine the font(s)
used to display text. 


<a name="qfont"></a>
### qt.QFont(arg) ###
<a name="qfontfromtable"></a>

Expression `qt.QFont(arg)` returns a new font object.
Argument `arg` may be a string previously returned
by [qfont:tostring()](#qfonttostring) or
a table whose fields specify the desired font characteristics. 
The following fields are recognized, listed in order of priority:

  * String field `family` may contain a comma separated list of the desired font families.  
  * Setting boolean field `sans` to `true` indicates a preference for sans serif font families such as "Helvetica". This is more portable than specifying a family.
  * Setting boolean field `serif` to `true` indicates a preference for serif font families such as "Times". This is more portable than specifying a family.
  * Setting boolean field `typewriter` to `true` indicates a preference for a fixed width font families such as "Courier". This is more portable than specifying a family.
  * Numerical field `size` specify the desired size of the font. The default size is 10 points.
  * Numerical fields `pixelSize` and `pointSize` also indicates the desired size of the font. Field `pixelSize` is an alias for `size`. Field `pointSize` includes a correction factor corresponding to the actual resolution of the target device. When both are precised, `pointSize` takes precedence. 
  * Numerical field `weight` defines the weight of the font. Weight 50 corresponds to a normal font and weight 75 corresponds to a bold font. 
  * Numerical field `stretch` is a percentage applied to the width of all characters. Value 100 corresponds to a normal width. Value 150 increases all width by 50 percent.
  * Setting boolean field `bold` to `true` requests a bold font if the selected font family defines a bold variant.
  * Setting boolean field `italic` to `true` requests and italic font if the selected font family defines an italic or oblique variant.
  * Setting boolean field `fixedPitch` to `true` requests a fixed pitch font if the selected font family defines such a variant (rare).
  * Setting boolean field `underline` to `true` draws a line below all the character at a font-specified position.
  * Setting boolean field `overline` to `true` draws a line above all the character at a font-specified position.
  * Setting boolean field `strikeOut` to `true` draws a line that crosses the character at a font-specified position.

Example:
```lua
require 'qtwidget'
w=qtwidget.newwindow(300,300)
w:moveto(20,20)
w:setfont(qt.QFont{serif=true,size=12,italic=true})
w:show("foo")
w:setfont(qt.QFont{serif=true,size=12,italic=true,underline=true})
w:setcolor("blue")
w:show("bar")
```


<a name="qfont.totable"></a>
### qfont:totable() ###

Expression `qfont:totable()` returns a table
suitable for use with [qt.QFont(table)](#qfontfromtable).
All the supported fields are populated.

Example:
```lua
require 'qtwidget'
f=qt.QFont{serif=true,size=12,bold=true}
for k,v in pairs(f:totable()) do print(k,v) end 
```

<a name="qfonttostring"></a>
### qfont:tostring() ###
<a name="qfont.tostring"></a>

Expression `qfont:tostring()` returns a string 
representation of the font settings that is
suitable for storing in configuration files for instance.
Use [qt.QFont(string)](#qfontfromtable) to
reconstruct the corresponding font object.

<a name="qfontinfo"></a>
### qfont:info() ###
<a name="qfont.info"></a>

Expression `qfont:info()` returns a table describing
the actual font selected by the font matching algorithm
when one uses `qfont` to display text on the screen.
This table can be used as argument to 
function [qt.QFont()](#qfontfromtable)
as it uses the same keys.  
This is achieved using the Qt class
[QFontInfo](http://doc.trolltech.com/4.4/qfontinfo.html).
Fields `"underline"`, `"overline"`, `"strikeOut"`, and `"stretch"` 
are copied verbatim from the `qfont` object.

<a name="qfontdialog"></a>
## qt.QFontDialog ##

Qt class
[QFontDialog](http://doc.trolltech.com/4.4/qfontdialog.html) 
implements a dialog for selecting fonts.  
The recommended way to use this class is the static function `getFont`.

<a name="qfontdialog.getfont"></a>
### qt.QFontDialog.getFont([font],[parent, [caption]]) ###

Pops up a dialog for selecting a font.
All arguments are optional.
Argument `font` is the initial font selection.
Argument `parent` is the parent widget.
Argument `caption` is the dialog window title.
This function returns `nil` if the no font was selected.
Otherwise it returns a qvariant of type [qt.QFont](#qfont).


<a name="qgradient"></a>
## qt.QGradient ##

Qt class 
[QGradient](http://doc.trolltech.com/4.4/qgradient.html) 
represents a color gradient for use in [gradient brushes](#qbrush).
No specific Lua methods have been defined for this Qt variant class.


<a name="qicon"></a>
## qt.QIcon ##

Qt class 
[QIcon](http://doc.trolltech.com/4.4/qimage.html) 
provides scalable icons in different modes and states.

<a name="qicon"></a>
### qt.QIcon([arg]) ###

Returns a new icon.
Argument `arg` can be a qt value of 
type [qt.QImage](#qimage)
or [qt.QPixmap](#qpixmap).
Alternatively, argument `arg` may be a
file name that will be loaded on demand.


<a name="qimage"></a>
## qt.QImage ##

Qt class 
[QImage](http://doc.trolltech.com/4.4/qimage.html) 
represents a device independent off-screen image.
Such images are represented in Lua 
as Qt variants of class `qt.QImage`.

<a name="qimage"></a>
### qt.QImage(...) ###

There are several ways to construct a Qt variant of class `qt.QImage`.

__ `qt.QImage(w,h,monoflag)` __

Returns a new image of width `w` and height `h`. 
The image is bitonal when the optional boolean flag `monoflag` 
is `true`. Otherwise the image is a 32 bits RGBA image.

__ `qt.QImage(f,format)` __

Returns a new image obtained by reading the image file `f`.
Argument `f` can be a file name or a Lua file descriptor.
The [file format](#qimageformats) is determined 
by the optional string `format` or by the file name extension.


<a name="qimage.depth"></a>
### qimage:depth() ###

Expression `qimage:depth()` returns the depth of the image `qimage`.
A depth of 1 indicates a bitonal image.
A depth of 24 or 32 indicates a true color image.

<a name="qimage.rect"></a>
### qimage:rect() ###

Expression `qimage:rect()` returns a Qt variant of class
[qt.QRect](qtcore.md#qrect) representing the
boundaries of the image `qimage`.

<a name="qimage.save"></a>
### qimage:save(f,[format]) ###

Expression `qimage:save(f,format)` saves the image data into file `f`.
Argument `f` can be a file name or a Lua file descriptor.
The [file format](#qimageformats) is determined by the 
optional string `format` or by the file name extension.

<a name="qimage.size"></a>
### qimage:size() ###

Expression `qimage:size()` returns a Qt variant of class
[qt.QSize](qtcore.md#qsize) representing the
size of the image `qimage`.

<a name="qimage.formats"></a>
### qt.QImage.formats([string]) ###
<a name="qimageformats"></a>

Function `qt.QImage.formats` describes the 
supported formats for reading or saving files.
The optional `string` argument must be a string
starting with letter `"r"` or `"w"` to indicate if
one is interested in the supported formats for reading or
writing image files.  When this letter is followed by letter `"f"`,
the function returns a string suitable for selecting name
filters in a file dialog. Otherwise, the function
returns a `qt.QStringList` with the supported formats.



<a name="qkeysequence"></a>
## qt.QKeySequence ##

Qt class
[QKeySequence](http://doc.trolltech.com/4.4/qkeysequence.html) 
encapsulates a key sequence as used by shortcuts.

<a name="qkeysequence"></a>
### qt.QKeySequence(string) ###

Returns a Qt value of class `qt.QKeySequence` 
associated with the key combination described in string `string`.
Up to four key codes may be entered by separating 
them with commas, e.g. `"Alt+V,Ctrl+Down,A,Shift+Home"`.

<a name="qkeysequence.tostring"></a>
### qkeysequence:tostring() ###

Returns a string describing the keys in the key sequence.


<a name="qmainwindow"></a>
## qt.QMainWindow ##

Qt class 
[QMainWindow](http://doc.trolltech.com/4.4/qmainwindow.html) 
provides a main application window with menu, statusbar, toolbars
and dockable widgets. This is a subclass of [QWidget](#qwidget).

<a name="qmainwindow"></a>
### qt.QMainWindow([parent]) ###

Expression `qt.QMainWindow(parent)` returns a new `QMainWindow` instance.
The optional argument `parent` specifies the widget parent.
When argument `parent` is `nil` or not specified,
the new widget is owned by the Lua interpreter 
and is automatically destroyed when the garbage collector
determines that it is no longer referenced.

<a name="qmainwindow.centralWidget"></a>
### qmainwindow:centralWidget() ###

Expression `qmainwindow:centralWidget()` returns the
central widget managed by the main window.

<a name="qmainwindow.menuBar"></a>
### qmainwindow:menuBar() ###

Returns the main window's menu bar.
This function creates and returns an empty menu bar
if the menu bar does not exist yet.

<a name="qmainwindow.setcentralwidget"></a>
### qmainwindow:setCentralWidget(qwidget) ###

Sets the widget `qwidget` to be the main window's central widget.
The main window takes ownership of the widget pointer and 
deletes it at the appropriate time.

<a name="qmainwindow.setmenubar"></a>
### qmainwindow:setMenuBar(qmenubar) ###

Sets the menu bar for the main window to `qmenubar`.
The main window takes ownership of the menu bar and 
deletes it at the appropriate time.

<a name="qmainwindow.setstatusbar"></a>
### qmainwindow:setStatusBar(qstatusbar) ###

Sets the status bar for the main window to `qstatusbar`.
The main window takes ownership of the status bar and 
deletes it at the appropriate time.

<a name="qmainwindow.statusBar"></a>
### qmainwindow:statusBar() ###

Returns the main window's status bar.
This function creates and returns an empty status bar
if the status bar does not exist yet.


<a name="qmenu"></a>
## qt.QMenu ##

Qt class 
[QMenu](http://doc.trolltech.com/4.4/qmenu.html) 
provides a menu widget for use in menu bars, 
context menus, and other popup menus. 
This is a subclass of [qt.QWidget](#qwidget).

The most flexible way to populate a menu
consists of first creating [qt.QAction](#qaction) 
objects for all the menu items. 
These objects can then be inserted
into the menu using the method `addAction` defined by 
the superclass [QWidget](#qwidget).  

Function `qmenu:addLuaAction` provides a more convenient way 
to create menu items bound to specific lua functions.
See [qt.QMenuBar](#qmenubar) for an example.

<a name="qmenu.addLuaAction"></a>
### qmenu:addLuaAction([icon,]text[,keys[,statustip]][,function]) ###

Creates a new [QtLuaAction](#qaction) owned by the menu
and appends it to the menu as a new menu item.
This function returns the newly created action.

  * Argument `icon` is an optional [qt.QIcon](#qicon) for the action.
  * Argument `text` specifies the text for the menu item.
  * Argument `keys` can be either a [qt.QKeySequence](#qkeysequence)specifying the menu item shortcut, or a string representing the shortcut key name. 
  * Argument `statustip` is an optional string that is displayed in the statusbar when the menu item is highlighted.
Finally argument `function` is a Lua function that is 
connected to the action signal `triggered(bool)` and
therefore is called when the menu item is selected. 

<a name="qmenu.addMenu"></a>
### qmenu:addMenu(newqmenu) ###

Adds the menu `newqmenu` as a submenu of the menu `qmenu`
and returns the corresponding action object.

<a name="qmenu.addMenu"></a>
### qmenu:addMenu([icon,] text) ###

Creates a new submenu with the specified `icon` and `text`
and adds it into the menu `qmenu`. This function returns
the new menu as an object of class `qt.QMenu`.

<a name="qmenu.addSeparator"></a>
### qmenu:addSeparator() ###

Adds a separator into the menu
and returns the corresponding action object.

<a name="qmenu.clear"></a>
### qmenu:clear() ###

Removes all the menu items.
Actions owned by the menu and not shown 
in any other widget are deleted.

<a name="qmenu.exec"></a>
### qmenu:exec([qpoint[, defaultqaction]]) ###

Pops up the menu `qmenu` at position `qpoint` and returns 
the action object corresponding to the selected menu item. 
This function returns `nil` if no menu item was selected.
Argument `defaultqaction` specifies which action is located
below the mouse cursor when the menu is first shown.
Argument `qpoint` defaults to `qmenu.pos`.

Note that actions added with `qmenu:addLuaAction`
are automatically disabled while Lua is running.
Therefore such actions cannot be used with
`qmenu:exec` because they would all be disabled.

<a name="qmenu.insertMenu"></a>
### qmenu:insertMenu(beforeqaction, newqmenu) ###

Inserts the menu `newqmenu` as a submenu into the menu `qmenu`
and returns the corresponding action object.
The submenu is inserted before action `beforeqaction` 
or is appended when `beforeqaction` is null or invalid for this widget.

<a name="qmenu.insertSeparator"></a>
### qmenu:insertSeparator(beforeqaction) ###

Inserts a new separator before action `beforeqaction` 
and returns the corresponding action object.

<a name="qmenu.menuAction"></a>
### qmenu:menuAction() ###

Returns the action associated with the menu `qmenu`.

<a name="qmenubar"></a>
## qt,QMenuBar ##

Qt class 
[QMenuBar](http://doc.trolltech.com/4.4/qmenu.html) 
provides a horizontal menu bar for use in main windows.
This is a subclass of [qt.QWidget](#qwidget).

Example:
```lua
w === qt.QMainWindow()
w:setCentralWidget(...) -- do something smart here
menubar === w:menuBar()
filemenu === menubar:addMenu("&File")
filemenu:addLuaAction("&Open", "Ctrl+O", function() doOpen(w) end)
filemenu:addLuaAction("&Close", "Ctrl+W", function() w:close() end)
editmenu === menubar:addMenu("&Edit")
editmenu:addLuaAction("&Cu&t", "Ctrl+X", function() doCut(w) end)
editmenu:addLuaAction("&Copy", "Ctrl+C", function() doCopy(w) end)
editmenu:addLuaAction("&Paste", "Ctrl+V", function() doPaste(w) end)
w:show()
```

<a name="qmenubar.addMenu"></a>
### qmenubar:addMenu(qmenu) ###

Adds the menu `qmenu` as a menu of the menubar `qmenubar`
and returns the corresponding action object.

<a name="qmenubar.addMenu"></a>
### qmenubar:addMenu([icon,] text) ###

Creates a new menu with the specified `icon` and `text`
and adds it into the menubar `qmenubar`. This function returns
the new menu as an object of class `qt.QMenu`.

<a name="qmenubar.clear"></a>
### qmenubar:clear() ###

Removes all the menus and actions from the menu bar.
Actions owned by the menu and not shown 
in any other widget are deleted.

<a name="qmenubar.insertMenu"></a>
### qmenubar:insertMenu(beforeqaction, qmenu) ###

Inserts the menu `qmenu` into the menu bar `qmenubar`
and returns the corresponding action object.
The submenu is inserted before action `beforeqaction` 
or is appended when `beforeqaction` is null or invalid for this widget.

<a name="qmenubar.insertSeparator"></a>
### qmenubar:insertSeparator(beforeqaction) ###

Inserts a new separator before action `beforeqaction` 
and returns the corresponding action object.

<a name="qpainterpath"></a>
## qt.QPainterPath ##

Qt class 
[QPainterPath](http://doc.trolltech.com/4.4/qpainterpath.html) 
represents mathematical boundaries delimiting
regions that can be painted using [painter:fill](qtwidget.md#painterfill) 
or delimited using [painter:stroke](qtwidget.md#painterstroke).
No specific Lua methods have been defined for this Qt variant class.


<a name="qpixmap"></a>
## qt.QPixmap ##

Qt class 
[QPixmap](http://doc.trolltech.com/4.4/qpixmap.html) 
represents a server-side image containing device-dependent data.
No specific Lua methods have been defined for this Qt variant class.


<a name="qpen"></a>
## qt.QPen ##

Qt class 
[QPen](http://doc.trolltech.com/4.4/qpen.html) 
represents the painter settings that determine how 
lines are drawn.

<a name="qpen"></a>
### qt.QPen(table) ###
<a name="qpenfromtable"></a>

Expression `qt.QPen(table)` returns a pen whose settings
are determined by the fields of table `table`. 
The following fields are recognized:

  * Field `style` specifies the name of the [pen style](http://doc.trolltech.com/4.4/qt.html#PenStyle-enum). The default style is `SolidLine`.
  * Field `brush` contains a `qt.QBrush` value that defines the appearance of the painted areas.
  * Field `color` contains a `qt.QColor` value for the brush color. Setting this field is equivalent to setting field `brush` with a solid brush of the specified color. Field `brush` has precedence over this field.
  * Numerical field `width` contains the desired line width. A line width of zero indicates a cosmetic pen. This means that the pen width is always drawn one pixel wide, independent of the transformation on the painter.
  * Setting boolean field `cosmetic` to `true` indicates that the pen width has a constant width regardless of the transformation on the painter.
  * Field `style` specifies the name of the [pen style](http://doc.trolltech.com/4.4/qt.html#PenStyle-enum) which determines 
  * Field `joinStyle` specifies the name of the [pen join style](http://doc.trolltech.com/4.4/qt.html#PenJoinStyle-enum) which determines how lines meet with different angles. The default join style is `BevelJoin`.
  * Field `capStyle` specifies the name of the [pen cap style](http://doc.trolltech.com/4.4/qt.html#PenCapStyle-enum) which determines how lines end are drawn. The default cap style is `SquareCap`.
  * Numerical field `miterLimit` determines how far a [miter join](http://doc.trolltech.com/4.4/qpen.html#setMiterLimit) can extend from the join point. This is used to reduce artifacts between line joins where the lines are close to parallel.


<a name="qpen.totable"></a>
### qpen:totable() ###

Expression `qpen:totable()` returns a table describing the pen.
See the documentation of [qt.QPen(table)](#qpenfromtable)
for a description of the table fields.


<a name="qtransform"></a>
## qt.QTransform ##

Qt class 
[QTransform](http://doc.trolltech.com/4.4/qtransform.html) 
represents a 2D transformation of a coordinate system.
Transformation matrices are represented as Qt variants 
of class `qt.QTransform`.

<a name="qtransform"></a>
### qt.QTransform([table[,table]]) ###
<a name="qtransformfromtable"></a>

Expression `qt.QTransform()` constructs an identity transformation matrix.

Expression `qt.QTransform(table)` constructs a transformation matrix
whose matrix elements are initialized with the fields `m11`, `m12`, `m13`,
`m21`, `m22`, `m23`, `m31`, `m32`, and `m33` of table `table`.

Expression `qt.QTransform(fquad,tquad)` constructs a transformation matrix
that maps the quad `fquad` to the quad `tquad`. 
Both arguments `fquad` and `tquad` are table containing exactly four
[qt.QPointF](qtcore.md#qpoint) representing the four corners of the quad.

<a name="qtransform.totable"></a>
### qtransform:totable() ###

Expression `qtransform:totable()` returns a table suitable
for use with expression [qt.QTransform(table)](#qtransformfromtable).

<a name="qtransform.scaled"></a>
### qtransform:scaled(sx,sy) ###

Function `qtransform:scaled` returns a new transformation matrix
representing a coordinate system whose axes have been
scaled by coefficient `sx` and `sy`.  

<a name="qtransform.translated"></a>
### qtransform:translated(dx,dy) ###

Function `qtransform:translated` returns a new transformation matrix
representing a coordinate system whose origin has been
translated by `dx` units along the X axis and `dy` units along the Y axis.


<a name="qtransform.sheared"></a>
### qtransform:sheared(cx,cy) ###

Function `qtransform:sheared` returns a new transformation matrix
representing a coordinate system that has been sheared using coefficients
`cx` horizontally and `cy` vertically.


<a name="qtransform.rotated"></a>
### qtransform:rotated(angle,[axis,[unit]]) ###

Function `qtransform:rotated` returns a new transformation matrix 
representing a coordinate system rotated by `angle` units around the origin.
The optional string argument `axis` may be `XAxis`, `YAxis`, or `ZAxis` and
defaults to `ZAxis`. The optional string argument `unit` may 
be `Degrees` or `Radians`
and default to `Degrees`.

<a name="qtransform.inverted"></a>
### qtransform:inverted() ###

Function `qtransform:inverted` returns 
the inverse transform of its argument
or `nil` when it is not invertible.

<a name="qtransform.map"></a>
### qtransform:map(...) ###

Function `qtransform:map` applies 
a transformation to its argument and returns
a new qvariant of the same type.
The argument can be a `QPoint`, `QPointF`, `QLine`, `QLineF`, 
`QPolygon`, `QPolygonF`, `QRegion`, `QPainterPath`, 
`QRect`, or `QRectF`.  When the argument is a rectangle,
the function returns the bounding rectangle of the polygon 
representing the transformed rectangle.
This function also take as argument to reals representing
the coordinates of a point and return two reals
representing the transformed coordinates.

<a name="qwidget"></a>
## qt.QWidget ##

Qt class 
[QWidget](http://doc.trolltech.com/4.4/qwidget.html) 
is the base class of all graphical interface components.
All widgets inherit class `qt.QWidget` and
its superclass [qt.QObject](qtcore.md#qobject).

<a name="qwidget"></a>
### qt.QWidget([parent]) ###

Expression `qt.QWidget(parent)` returns a new widget.
The optional argument `parent` specifies the widget parent.
New widgets are always created from the main thread using
the [thread hopping](qt.md#qt.qcall) mechanism.

When argument `parent` is `nil` or not specified,
the new widget is owned by the Lua interpreter 
and is automatically destroyed when the garbage collector
determines that it is no longer referenced.


<a name="qwidget.actions"></a>
### qwidget:actions() ###

Expression `qwidget:actions()` returns a 
qt value of class [qt.QVariantList](qtcore.md#qvariantlist)
containing list of actions associated with this widget.

<a name="qwidget.addAction"></a>
### qwidget:addAction(qaction) ###

Appends action `qaction` to the 
list of actions associated with this widget.

Example: creating a context menu
```lua
action1 === qt.QtLuaAction{text="Checkme", checkable=true,
            function(b) print("checked:", b) end}
action2 === qt.QtLuaAction{text="Pickme",
            function() print("picked") end}
w:addAction(action1)
w:addAction(action2)
w.contextMenuPolicy='ActionsContextMenu'
```

<a name="qwidget.insertAction"></a>
### qwidget:insertAction(beforeqaction, qaction) ###

Insert action ==qaction== into the 
list actions associated with this widget.
Argument ==beforeqaction== indicates at which position
to insert the action. If ==beforeqaction== is omitted
or invalid, the action is simply appended.

<a name="qwidget.mapToGlobal"></a>
### qwidget:mapToGlobal(qpoint) ###

Translates the widget coordinates `qpoint` to global screen coordinates.
Both argument `qpoint` and the return value are Qt value of class
[qt.QPoint](qtcore.md#qpoint).

<a name="qwidget.mapFromGlobal"></a>
### qwidget:mapFromGlobal(qpoint) ###

Translates the global screen coordinates `qpoint` to widget coordinates.
Both argument `qpoint` and the return value are Qt value of class
[qt.QPoint](qtcore.md#qpoint).    


<a name="qwidget.removeAction"></a>
### qwidget:removeAction(qaction) ###

Removes action `qaction` from the 
list actions associated with this widget.


<a name="qwidget.render"></a>
### qwidget:render([pointer]) ###

When called without argument, 
this function draws the widget into an image
and returns the image.

The optional argument `pointer` can be a 
pointer to a `QPainter` or a `QPaintDevice`
where the widget should be rendered.
Such pointers can be obtained using functions 
[painter:painter](qtwidget.md#painterpainter) or
[painter:device](qtwidget.md#painterdevice).
When such an argument is specified,
nothing is returned.

<a name="qwidget.setAttribute"></a>
### qwidget:setAttribute(widgetattribute,[value]) ###

Sets the specified
[widget attribute](http://doc.trolltech.com/4.4/qt.html#WidgetAttribute-enum)
to boolean value `value`.
Some of these flags are used internally by Qt. 
Care is required.

<a name="qwidget.setWindowFlag"></a>
### qwidget:setWindowFlag(windowflag,[value]) ###

Sets the specified
[window flag](http://doc.trolltech.com/4.4/qt.html#WindowType-enum)
to boolean value `value`.
Flags indicating a window type 
are handled as exclusive flags.
Setting these flags usually hides the window.
You need to do call `qwidget:show` again.

<a name="qwidget.testAttribute"></a>
### qwidget:testAttribute(widgetattribute) ###

Returns a boolean indicating the value of the specified
[widget attribute](http://doc.trolltech.com/4.4/qt.html#WidgetAttribute-enum).
Flags indicating a window type 
are handled as exclusive flags.

<a name="qwidget.testWindowFlag"></a>
### qwidget:testWindowFlag(windowflag) ###

Returns a boolean indicating the value of the specified
[window flag](http://doc.trolltech.com/4.4/qt.html#WindowType-enum).
Flags indicating a window type 
are handled as exclusive flags.

<a name="qwidget.window"></a>
### qwidget:window() ###

Expression `qwidget:window()` returns the window for widget `qwidget`,
that is the next ancestor that is (or could be) displayed
with a window frame.

For instance the following code changes the
title of the window containing widget `qwidget`.

```lua
qwidget:window().windowTitle "New Title"
```




