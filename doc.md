N-Puzzle Design Document.
-------------------------
### Pieter van Dijk
### 10252088	

The App's layout: 

![My Picture](https://github.com/phhm/appstudio/blob/master/app-idea.png)


**The purpose of this app** is to create a game for android-operation mobile devices. In this game, a picture will be divided in multiple tiles, which are subsequently shuffled. 1 of the tiles is left away, in order to be able to move tiles to that empty position. When the original place of all tiles has been found, the game is finished. Several pictures can be chosen to use as puzzle.

**In this first view**, the public class `MainActivity` (1. in the layout picture), besides some welcoming text, a list of possible images to use for the puzzle is shown. The data of this pictures can be displayed by `ListView`, whereas the actual data will be hold by `ArrayAdapter`.

`photos = new ArrayAdapter(Context c, int resource, List objects)`  
`getListView().setListAdapter(photos)`

the last sentence visualizes the data that was stored in ‘photos’.

The OnClickListener method is implemented, in order to make a certain action happen when of of the List elements is clicked on.

**To be able to use images in the app**, they should be stored as Bitmaps. This can be done by using BitmapFactory. In the res/drawable folder are the image files located.

`Bitmap photo1 = BitmapFactory.decodeResource(Resources getResources(), R.drawable.<image>)`  
`ImageView imageView = new ImageView(this);`  
`imageView.setImageBitmap(photo1)`

`LinearLayout root = (Linearlayout)this.findViewById(R.id.root_layout);`  
`root.addView(imageView)`

Here, in public class `SecondActivity` (2. in the app-layout image), the bitmap is shown to the screen

After 3 seconds, the image has to be divided in tiles, this can be done by cropping the image in multiple pieces. By creating multiple bitmaps with a given width/height, this can be done.

`Bitmap.createBitmap(Bitmap bitmap, int x, inty, (this.getResources().getDisplayMetrics().widthPixels)/difficulty, (this.getResources().getDisplayMetrics().heightPixels)/difficulty`

In this way, multiple images are created, where int x and int y can be set as points (in left-top corner of sub-image) that depend on the sub-image that was created before (for example in a for-loop). The width and height of the created sub-images depend on the width and height of the image and the difficulty level (so 3 for easy, 4 for medium and 5 for hard).
In public class `ThirdActivity` (3. in the app-layout image)

The different images will all be linked to a number, from 1-8 (easy), 1-15(medium) or 1-24 (hard).
Whenever a certain tile is pushed, it will go to the empty adjecent tile and the board will change. When the numbers are ordered from 1-highest number (left top to right bottom) the game is finished.

**Whenever the game is left** (no more battery, or user presses the home button) the game should be saved. This means that the state of the board, the number of moves and the difficulty should be saved.The public void `super.onPause()` method should be called whenever the Activity (Main-, Second- or Third) is removed from foreground state. In this function should the ‘saves’ take place:

`sharedPreferencesEditor.put<type>(String name, int value)`

in which a type can be a boolean, string, int etc…, the name is the key of the value, and the int value is the value that is returned when the name is not found.

**When each activity is created**, `super.onCreate()` is created first.
Right before an Activity is put in the foreground state (again) `super.onResume()` should be called: here the saved key/value pares are to get back again:

`sharedPreferences.get<type>(String name, int value)`

Also, as visible in 4. in the app-layout image, a menu should appear when the menu button is pressed. This menu should be put in the `res/menu` folder, where `<menu>` has multiple `<item>` children.
A menu is a xml file. When the user requests to open the menu, the `onCreateOptionsMenu(Menu menu)` method is fired.

**the Public Class MenuInflater** inside of `MainActivity` instantiates the menu XML-files into Menu objects. With the `onOptionsItemSelected(MenuItem item)` to be fired when a menu item is selected. This item can be identified with the `item.getItemId()` -method, which returns the Id that was specified in the menu-XML file.  
In case of 5. in the app-layout image, the difficulty is selected, which opens a new Activity, `FourthActivity`, in where from a list a difficulty can be chosen. After this the game is reinstantiated with another difficulty level.

