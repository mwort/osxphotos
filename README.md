# OSXPhotos

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/python/black)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

- [OSXPhotos](#osxphotos)
  * [What is osxphotos?](#what-is-osxphotos)
  * [Supported operating systems](#supported-operating-systems)
  * [Installation instructions](#installation-instructions)
  * [Command Line Usage](#command-line-usage)
  * [Example uses of the module](#example-uses-of-the-module)
  * [Module Interface](#module-interface)
    + [PhotosDB](#photosdb)
      - [Open the default Photos library](#open-the-default-photos-library)
      - [Open System Photos library](#open-system-photos-library)
      - [Open a specific Photos library](#open-a-specific-photos-library)
      - [`keywords`](#keywords)
      - [`albums`](#albums)
      - [`albums_shared`](#albums_shared)
      - [`persons`](#persons)
      - [`keywords_as_dict`](#keywords_as_dict)
      - [`persons_as_dict`](#persons_as_dict)
      - [`albums_as_dict`](#albums_as_dict)
      - [`albums_shared_as_dict`](#albums_shared_as_dict)
      - [`library_path`](#library_path)
      - [`db_path`](#db_path)
      - [`db_version`](#db_version)
      - [` photos(keywords=None, uuid=None, persons=None, albums=None, images=True, movies=False)`](#-photoskeywordsnone-uuidnone-personsnone-albumsnone-imagestrue-moviesfalse)
    + [PhotoInfo](#photoinfo)
      - [`uuid`](#uuid)
      - [`filename`](#filename)
      - [`original_filename`](#original_filename)
      - [`date`](#date)
      - [`description`](#description)
      - [`title`](#title)
      - [`keywords`](#keywords-1)
      - [`albums`](#albums-1)
      - [`persons`](#persons-1)
      - [`path`](#path)
      - [`path_edited`](#path_edited)
      - [`ismissing`](#ismissing)
      - [`hasadjustments`](#hasadjustments)
      - [`external_edit`](#external_edit)
      - [`favorite`](#favorite)
      - [`hidden`](#hidden)
      - [`location`](#location)
      - [`shared`](#shared)
      - [`isphoto`](#isphoto)
      - [`ismovie`](#ismovie)
      - [`iscloudasset`](#iscloudasset)
      - [`incloud`](#incloud)
      - [`uti`](#uti)
      - [`burst`](#burst)
      - [`burst_photos`](#burst_photos)
      - [`live_photo`](#live_photo)
      - [`path_live_photo`](#path_live_photo)
      - [`json()`](#json)
      - [`export(dest, *filename, edited=False, overwrite=False, increment=True, sidecar=False)`](#exportdest-filename-editedfalse-overwritefalse-incrementtrue-sidecarfalse)
    + [Utility Functions](#utility-functions)
      - [```get_system_library_path()```](#get_system_library_path)
      - [```get_last_library_path()```](#get_last_library_path)
      - [```list_photo_libraries()```](#list_photo_libraries)
      - [```dd_to_dms_str(lat, lon)```](#dd_to_dms_strlat-lon)
      - [```create_path_by_date(dest, dt)```](#create_path_by_datedest-dt)
    + [Examples](#examples)
  * [Related Projects](#related-projects)
  * [Contributing](#contributing)
  * [Implementation Notes](#implementation-notes)
  * [Dependencies](#dependencies)
  * [Acknowledgements](#acknowledgements)

## What is osxphotos?

OSXPhotos provides the ability to interact with and query Apple's Photos.app library database on MacOS. Using this module you can query the Photos database for information about the photos stored in a Photos library on your Mac--for example, file name, file path, and metadata such as keywords/tags, persons/faces, albums, etc. You can also easily export both the original and edited photos.

## Supported operating systems

Only works on MacOS (aka Mac OS X). Tested on MacOS 10.12.6 / Photos 2.0, 10.13.6 / Photos 3.0, MacOS 10.14.5, 10.14.6 / Photos 4.0, MacOS 10.15.1 / Photos 5.0. Requires python >= 3.6

This module will read Photos databases for any supported version on any supported OS version.  E.g. you can read a database created with Photos 4.0 on MacOS 10.14 on a machine running MacOS 10.12


## Installation instructions

osxmetadata uses setuptools, thus simply run:

	python3 setup.py install

## Command Line Usage

This module will install a command line utility called `osxphotos` that allows you to query the Photos database.  Alternatively, you can also run the command line utility like this: `python3 -m osxphotos`

If you only care about the command line tool, I recommend installing with [pipx](https://github.com/pipxproject/pipx)

After installing pipx:
`pipx install osxphotos`

Then you should be able to run `osxphotos` on the command line:

```
> osxphotos
Usage: osxphotos [OPTIONS] COMMAND [ARGS]...

Options:
  --db <Photos database path>  Specify database file.
  --json                       Print output in JSON format.
  -v, --version                Show the version and exit.
  -h, --help                   Show this message and exit.

Commands:
  albums    Print out albums found in the Photos library.
  dump      Print list of all photos & associated info from the Photos...
  export    Export photos from the Photos database.
  help      Print help; for help on commands: help <command>.
  info      Print out descriptive info of the Photos library database.
  keywords  Print out keywords found in the Photos library.
  list      Print list of Photos libraries found on the system.
  persons   Print out persons (faces) found in the Photos library.
  query     Query the Photos database using 1 or more search options; if...
```

To get help on a specific command, use `osxphotos help <command_name>`

Example: `osxphotos help export`

```
Usage: osxphotos help [OPTIONS] DEST

  Export photos from the Photos database. Export path DEST is required.
  Optionally, query the Photos database using 1 or more search options;  if
  more than one option is provided, they are treated as "AND"  (e.g. search
  for photos matching all options). If no query options are provided, all
  photos will be exported.

Options:
  --keyword TEXT      Search for keyword(s).
  --person TEXT       Search for person(s).
  --album TEXT        Search for album(s).
  --uuid TEXT         Search for UUID(s).
  --title TEXT        Search for TEXT in title of photo.
  --no-title          Search for photos with no title.
  --description TEXT  Search for TEXT in description of photo.
  --no-description    Search for photos with no description.
  --uti TEXT          Search for photos whose uniform type identifier (UTI)
                      matches TEXT
  -i, --ignore-case   Case insensitive search for title or description. Does
                      not apply to keyword, person, or album.
  --edited            Search for photos that have been edited.
  --external-edit     Search for photos edited in external editor.
  --favorite          Search for photos marked favorite.
  --not-favorite      Search for photos not marked favorite.
  --hidden            Search for photos marked hidden.
  --not-hidden        Search for photos not marked hidden.
  --burst             Search for photos that were taken in a burst.
  --not-burst         Search for photos that are not part of a burst.
  --live              Search for Apple live photos
  --not-live          Search for photos that are not Apple live photos
  --shared            Search for photos in shared iCloud album (Photos 5
                      only).
  --not-shared        Search for photos not in shared iCloud album (Photos 5
                      only).
  -V, --verbose       Print verbose output.
  --overwrite         Overwrite existing files. Default behavior is to add
                      (1), (2), etc to filename if file already exists. Use
                      this with caution as it may create name collisions on
                      export. (e.g. if two files happen to have the same name)
  --export-by-date    Automatically create output folders to organize photos
                      by date created (e.g. DEST/2019/12/20/photoname.jpg).
  --export-edited     Also export edited version of photo if an edited version
                      exists.  Edited photo will be named in form of
                      "photoname_edited.ext"
  --export-bursts     If a photo is a burst photo export all associated burst
                      images in the library.
  --export-live       If a photo is a live photo export the associated live
                      video component.  Live video will have same name as 
                      photo but with .mov extension.
  --original-name     Use photo's original filename instead of current
                      filename for export.
  --sidecar           Create json sidecar for each photo exported in format
                      useable by exiftool (https://exiftool.org/) The sidecar
                      file can be used to apply metadata to the file with
                      exiftool, for example: "exiftool -j=photoname.jpg.json
                      photoname.jpg" The sidecar file is named in format
                      photoname.ext.json where ext is extension of the photo
                      (e.g. jpg).
  --only-movies       Search only for movies (default searches both images and
                      movies).
  --only-photos       Search only for photos/images (default searches both
                      images and movies).
  -h, --help          Show this message and exit.
```

Example: find all photos with keyword "Kids" and output results to json file named results.json:

`osxphotos query --keyword Kids --json >results.json`

## Example uses of the module 

```python
import osxphotos

def main():
    photosdb = osxphotos.PhotosDB()
    print(photosdb.keywords)
    print(photosdb.persons)
    print(photosdb.albums)

    print(photosdb.keywords_as_dict)
    print(photosdb.persons_as_dict)
    print(photosdb.albums_as_dict)

    # find all photos with Keyword = Foo and containing John Smith
    photos = photosdb.photos(keywords=["Foo"],persons=["John Smith"])

    # find all photos that include Alice Smith but do not contain the keyword Bar
    photos = [p for p in photosdb.photos(persons=["Alice Smith"]) 
                if p not in photosdb.photos(keywords=["Bar"]) ]
    for p in photos:
        print(
            p.uuid,
            p.filename,
            p.original_filename,
            p.date,
            p.description,
            p.title,
            p.keywords,
            p.albums,
            p.persons,
            p.path,
        )

if __name__ == "__main__":
    main()
```

```python
""" Export all photos to ~/Desktop/export
    If file has been edited, export the edited version, 
    otherwise, export the original version """

import os.path

import osxphotos


def main():
    photosdb = osxphotos.PhotosDB()
    photos = photosdb.photos()

    export_path = os.path.expanduser("~/Desktop/export")

    for p in photos:
        if not p.ismissing:
            if p.hasadjustments:
                exported = p.export(export_path, edited=True)
            else:
                exported = p.export(export_path)
            print(f"Exported {p.filename} to {exported}")
        else:
            print(f"Skipping missing photo: {p.filename}")


if __name__ == "__main__":
    main()
```

## Module Interface

### PhotosDB

#### Open the default Photos library

```python
osxphotos.PhotosDB()
osxphotos.PhotosDB(path)
osxphotos.PhotosDB(dbfile=path)
```

Opens the Photos library database and returns a PhotosDB object.  

Optionally, pass the path to a specific database file or a Photos library (e.g. "/Users/smith/Pictures/Photos Library.photoslibrary" or "/Users/smith/Pictures/Photos Library.photoslibrary/database/photos.db").  Path to photos library may be passed **either** as first argument **or** as named argument `dbfile`. If path is not passed, PhotosDB will attempt to open the default Photos library (that is, the last library that was opened in Photos.app which may or may not also be the System Photos Library). **Note**: Users may specify a different library to open by holding down the *option* key while opening Photos.app. 

If an invalid path is passed, PhotosDB will raise `ValueError` exception.

Open the default (last opened) Photos library. (E.g. this is the library that would open if the user opened Photos.app)

```python
import osxphotos

photosdb = osxphotos.PhotosDB()
```

#### Open System Photos library

In Photos 5 (Catalina / MacOS 10.15), you can use `get_system_library_path()` to get the path to the System photo library if you want to ensure PhotosDB opens the system library.  This does not work on older versions of MacOS. E.g.

```python
import osxphotos

path = osxphotos.get_system_library_path()
photosdb = osxphotos.PhotosDB(path)
```

also,

```python
import osxphotos

path = osxphotos.get_system_library_path()
photosdb = osxphotos.PhotosDB(dbfile=path)
```

#### Open a specific Photos library
```python
import osxphotos

photosdb = osxphotos.PhotosDB(dbfile="/Users/smith/Pictures/Test.photoslibrary/database/photos.db")
```

or

```python
import osxphotos

photosdb = osxphotos.PhotosDB("/Users/smith/Pictures/Test.photoslibrary")
```

Pass the fully qualified path to the Photos library or the actual database file inside the library. The database is called photos.db and resides in the database folder in your Photos library.  If you pass only the path to the library, PhotosDB will add the database path automatically.  The option to pass the actual database path is provided so database files can be queried even if separated from the actual .photoslibrary file. 

Returns a PhotosDB object. 

**Note**: If you have a large library (e.g. many thousdands of photos), creating the PhotosDB object can take a long time (10s of seconds).  See [Implementation Notes](#implementation-notes) for additional details. 

#### `keywords`
```python
# assumes photosdb is a PhotosDB object (see above)
keywords = photosdb.keywords
```

Returns a list of the keywords found in the Photos library

#### `albums`
```python
# assumes photosdb is a PhotosDB object (see above)
albums = photosdb.albums
```

Returns a list of the albums found in the Photos library.  

**Note**: In Photos 5.0 (MacOS 10.15/Catalina), It is possible to have more than one album with the same name in Photos.  Albums with duplicate names are treated as a single album and the photos in each are combined.  For example, if you have two albums named "Wedding" and each has 2 photos, osxphotos will treat this as a single album named "Wedding" with 4 photos in it.

#### `albums_shared`

Returns list of shared albums found in photos database (e.g. albums shared via iCloud photo sharing)

**Note**: *Only valid for Photos 5 / MacOS 10.15*; on Photos <= 4, prints warning and returns empty list.

#### `persons`
```python
# assumes photosdb is a PhotosDB object (see above)
persons = photosdb.persons
```

Returns a list of the persons (faces) found in the Photos library

#### `keywords_as_dict`
```python
# assumes photosdb is a PhotosDB object (see above)
keyword_dict = photosdb.keywords_as_dict
```

Returns a dictionary of keywords found in the Photos library where key is the keyword and value is the count of how many times that keyword appears in the library (ie. how many photos are tagged with the keyword).  Resulting dictionary is in reverse sorted order (e.g. keyword with the highest count is first).

#### `persons_as_dict`
```python
# assumes photosdb is a PhotosDB object (see above)
persons_dict = photosdb.persons_as_dict
```

Returns a dictionary of persons (faces) found in the Photos library where key is the person name and value is the count of how many times that person appears in the library (ie. how many photos are tagged with the person).  Resulting dictionary is in reverse sorted order (e.g. person who appears in the most photos is listed first).

#### `albums_as_dict`
```python
# assumes photosdb is a PhotosDB object (see above)
albums_dict = photosdb.albums_as_dict
```

Returns a dictionary of albums found in the Photos library where key is the album name and value is the count of how many photos are in the album.  Resulting dictionary is in reverse sorted order (e.g. album with the most photos is listed first).  

**Note**: In Photos 5.0 (MacOS 10.15/Catalina), It is possible to have more than one album with the same name in Photos.  Albums with duplicate names are treated as a single album and the photos in each are combined.  For example, if you have two albums named "Wedding" and each has 2 photos, osxphotos will treat this as a single album named "Wedding" with 4 photos in it.

#### `albums_shared_as_dict`
```python
# assumes photosdb is a PhotosDB object (see above)
albums_shared_dict = photosdb.albums_shared_as_dict
```

Returns a dictionary of shared albums (e.g. shared via iCloud photo sharing) found in the Photos library where key is the album name and value is the count of how many photos are in the album.  Resulting dictionary is in reverse sorted order (e.g. album with the most photos is listed first).   

**Note**: *Photos 5 / MacOS 10.15 only*.  On earlier versions of Photos, prints warning and returns empty dictionary.

#### `library_path`
```python
# assumes photosdb is a PhotosDB object (see above)
photosdb.library_path
```

Returns the path to the Photos library as a string

#### `db_path`
```python
# assumes photosdb is a PhotosDB object (see above)
photosdb.db_path
```

Returns the path to the Photos database PhotosDB was initialized with

#### `db_version`
```python
# assumes photosdb is a PhotosDB object (see above)
photosdb.db_version
```

Returns the version number for Photos library database.  You likely won't need this but it's provided in case needed for debugging. PhotosDB will print a warning to `sys.stderr` if you open a database version that has not been tested. 


#### ` photos(keywords=None, uuid=None, persons=None, albums=None, images=True, movies=False)`

```python
# assumes photosdb is a PhotosDB object (see above)
photos = photosdb.photos([keywords=['keyword',]], [uuid=['uuid',]], [persons=['person',]], [albums=['album',]])
```

Returns a list of [PhotoInfo](#PhotoInfo) objects.  Each PhotoInfo object represents a photo in the Photos Libary.

If called with no parameters, returns a list of every photo in the Photos library. 

May be called with one or more of the following parameters:
```python
photos = photosdb.photos(
    keywords = [],
    uuid = [],
    persons = [],
    albums = [],
    images = bool,
    movies = bool,
)
```

- ```keywords```: list of one or more keywords.  Returns only photos containing the keyword(s).  If more than one keyword is provided finds photos matching any of the keywords (e.g. treated as "or")
- ```uuid```: list of one or more uuids.  Returns only photos whos UUID matches.  **Note**: The UUID is the universally unique identifier that the Photos database uses to identify each photo.  You shouldn't normally need to use this but it is a way to access a specific photo if you know the UUID.  If more than more uuid is provided, returns photos that match any of the uuids (e.g. treated as "or")
- ```persons```: list of one or more persons. Returns only photos containing the person(s).  If more than one person provided, returns photos that match any of the persons (e.g. treated as "or")
- ```albums```: list of one or more album names.  Returns only photos contained in the album(s). If more than one album name is provided, returns photos contained in any of the albums (.e.g. treated as "or")
- ```images```: bool; if True, returns photos/images; default is True
- ```movies```: bool; if True, returns movies/videos; default is False

If more than one of (keywords, uuid, persons, albums) is provided, they are treated as "and" criteria. E.g.

Finds all photos with (keyword = "wedding" or "birthday") and (persons = "Juan Rodriguez")

```python
photos=photosdb.photos(keywords=["wedding","birthday"],persons=["Juan Rodriguez"])
```

Find all photos tagged with keyword "wedding":
```python
# assumes photosdb is a PhotosDB object (see above)
photos = photosdb.photos(keywords=["wedding"])
 ```

Find all photos of Maria Smith
```python
# assumes photosdb is a PhotosDB object (see above)
photos=photosdb.photos(persons=["Maria Smith"])
```

Find all photos in album "Summer Vacation" or album "Ski Trip"
```python
# assumes photosdb is a PhotosDB object (see above)
photos=photosdb.photos(albums=["Summer Vacation", "Ski Trip"])
```

Find the single photo with uuid = "osMNIO5sQFGZTbj9WrydRB"
```python
# assumes photosdb is a PhotosDB object (see above)
photos=photosdb.photos(uuid=["osMNIO5sQFGZTbj9WrydRB"])
```

If you need to do more complicated searches, you can do this programmaticaly.  For example, find photos with keyword = "Kids" but not in album "Vacation 2019" 

```python
# assumes photosdb is a PhotosDB object (see above)
photos1 = photosdb.photos(albums=["Vacation 2019"])
photos2 = photosdb.photos(keywords=["Kids"])
photos3 = [p for p in photos2 if p not in photos1]
```

By default, photos() only returns images, not movies.  To also get movies, pass movies=True:
```python
photos_and_movies = photosdb.photos(movies=True)
```

To get only movies:
```python
movies = photosdb.photos(images=False, movies=True)
```
**Note** PhotosDB.photos() may return a different number of photos than Photos.app reports in the GUI. This is because photos() returns [hidden](#hidden) photos, [shared](#shared) photos, and for [burst](#burst) photos, all selected burst images even if non-selected burst images have not been deleted. Photos only reports 1 single photo for each set of burst images until you "finalize" the burst by selecting key photos and deleting the others using the "Make a selection" option. 

For example, in my library, Photos says I have 19,386 photos and 474 movies.  However, PhotosDB.photos() reports 25,002 photos.  The difference is due to 5,609 shared photos and 7 hidden photos.  (*Note* Shared photos only valid for Photos 5).  Similarly, filtering for just movies returns 625 results.  The difference between 625 and 474 reported by Photos is due to 151 shared movies.

```python
>>> import osxphotos
>>> photosdb = osxphotos.PhotosDB()
>>> photos = photosdb.photos()
>>> len(photos)
25002
>>> shared = [p for p in photos if p.shared]
>>> len(shared)
5609
>>> not_shared = [p for p in photos if not p.shared]
>>> len(not_shared)
19393
>>> hidden = [p for p in photos if p.hidden]
>>> len(hidden)
7
>>> movies = photosdb.photos(movies=True, images=False)
>>> len(movies)
625
>>> shared_movies = [m for m in movies if m.shared]
>>> len(shared_movies)
151
>>>
```

### PhotoInfo 
PhotosDB.photos() returns a list of PhotoInfo objects.  Each PhotoInfo object represents a single photo in the Photos library.

#### `uuid`
Returns the universally unique identifier (uuid) of the photo.  This is how Photos keeps track of individual photos within the database.

#### `filename`
Returns the current filename of the photo on disk.  See also [original_filename](#original_filename)

#### `original_filename`
Returns the original filename of the photo when it was imported to Photos.  **Note**: Photos 5.0+ renames the photo when it adds the file to the library using UUID.  See also [filename](#filename)

#### `date`
Returns the date of the photo as a datetime.datetime object

#### `description`
Returns the description of the photo

#### `title`
Returns the title of the photo

#### `keywords`
Returns a list of keywords (e.g. tags) applied to the photo

#### `albums`
Returns a list of albums the photo is contained in

#### `persons`
Returns a list of the names of the persons in the photo

#### `path`
Returns the absolute path to the photo on disk as a string.  **Note**: this returns the path to the *original* unedited file (see [hasadjustments](#hasadjustments)).  If the file is missing on disk, path=`None` (see [ismissing](#ismissing)).

#### `path_edited`
Returns the absolute path to the edited photo on disk as a string.  If the photo has not been edited, returns `None`.  See also [path](#path) and [hasadjustments](#hasadjustments).  

**Note**: will also return None if the edited photo is missing on disk. 

#### `ismissing`
Returns `True` if the original image file is missing on disk, otherwise `False`.  This can occur if the file has been uploaded to iCloud but not yet downloaded to the local library or if the file was deleted or imported from a disk that has been unmounted and user hasn't enabled "Copy items to the Photos library" in Photos preferences. **Note**: this status is computed based on data in the Photos library and `ismissing` does not verify if the photo is actually missing. See also [path](#path).

#### `hasadjustments`
Returns `True` if the picture has been edited, otherwise `False`

#### `external_edit`
Returns `True` if the picture was edited in an external editor (outside Photos.app), otherwise `False`

#### `favorite`
Returns `True` if the picture has been marked as a favorite, otherwise `False`

#### `hidden`
Returns `True` if the picture has been marked as hidden, otherwise `False`

#### `location`
Returns latitude and longitude as a tuple of floats (latitude, longitude).  If location is not set, latitude and longitude are returned as `None`

#### `shared`
Returns True if photo is in a shared album, otherwise False.

**Note**: *Only valid on Photos 5 / MacOS 10.15*; on Photos <= 4, returns None instead of True/False. 

#### `isphoto`
Returns True if type is photo/still image, otherwise False

#### `ismovie`
Returns True if type is movie/video, otherwise False

#### `iscloudasset`
Returns True if photo is a cloud asset, that is, it is in a library synched to iCloud.  See also [incloud](#incloud)

#### `incloud`
Returns True if photo is a [cloud asset](#iscloudasset) and is synched to iCloud otherwise False if photo is a cloud asset and not yet synched to iCloud. Returns None if photo is not a cloud asset.

**Note**: Applies to master (original) photo only.  It's possible for the master to be in iCloud but a local edited version is not yet synched to iCloud. `incloud` provides status of only the master photo.  osxphotos does not yet provide a means to determine if the edited version is in iCloud.  If you need this feature, please open a [issue](https://github.com/RhetTbull/osxphotos/issues)

#### `uti`
Returns Uniform Type Identifier (UTI) for the image, for example: 'public.jpeg' or 'com.apple.quicktime-movie'

#### `burst`
Returns True if photos is a burst image (e.g. part of a set of burst images), otherwise False.
See [burst_photos](#burst_photos)

#### `burst_photos`
If photo is a burst image (see [burst](#burst)), returns a list of PhotoInfo objects for all other photos in the same burst set. If not a burst image, returns empty list.

Example below gets list of all photos that are bursts, selects one of of them and prints out the names of the other images in the burst set.  PhotosDB.photos() will only return the photos in the burst set that the user [selected](https://support.apple.com/guide/photos/view-photo-bursts-phtde06a275d/mac) using "Make a Selection..." in Photos or the key image Photos selected if the user has not yet made a selection.  This is similar to how Photos displays and counts burst photos.  Using `burst_photos` you can access the other images in the burst set to export them, etc. 

```python
>>> import osxphotos
>>> photosdb = osxphotos.PhotosDB()
>>> bursts = [p for p in photosdb.photos() if p.burst]
>>> burst_photo = bursts[5]
>>> len(burst_photo.burst_photos)
4
>>> burst_photo.original_filename
'IMG_9851.JPG'
>>> for photo in burst_photo.burst_photos:
...     print(photo.original_filename)
...
IMG_9853.JPG
IMG_9852.JPG
IMG_9854.JPG
IMG_9855.JPG
```

#### `live_photo`
Returns True if photo is an Apple live photo (ie. it has an associated "live" video component), otherwise returns False.  See [path_live_photo](#path_live_photo).

#### `path_live_photo`
Returns the path to the live video component of a [live photo](#live_photo). If photo is not a live photo, returns None.

**Note**: will also return None if the live video component is missing on disk. It's possible that the original photo may be on disk ([ismissing](#ismissing)==False) but the video component is missing, likely because it has not been downloaded from iCloud.

#### `json()`
Returns a JSON representation of all photo info 

#### `export(dest, *filename, edited=False, overwrite=False, increment=True, sidecar=False)`

Export photo from the Photos library to another destination on disk.  
- dest: must be valid destination path as str (or exception raised).
- *filename (optional): name of picture as str; if not provided, will use current filename
- edited: boolean; if True (default=False), will export the edited version of the photo (or raise exception if no edited version)
- overwrite: boolean; if True (default=False), will overwrite files if they alreay exist
- increment: boolean; if True (default=True), will increment file name until a non-existant name is found
- sidecar: boolean; if True (default=False) will also write a json sidecar file with EXIF data in format readable by [exiftool](https://exiftool.org/); filename will be dest/filename.ext.json where ext is suffix of the image file (e.g. jpeg or jpg)

The json sidecar file can be used by exiftool to apply the metadata from the json file to the image.  For example: 

```python
import osxphotos

photosdb = osxphotos.PhotosDB()
photos = photosdb.photos()
photos[0].export("/tmp","photo_name.jpg",sidecar=True)
```

Then

`exiftool -j=photo_name.jpg.json photo_name.jpg`

If overwrite=False and increment=False, export will fail if destination file already exists

Returns the full path to the exported file

**Implementation Note**: Because the usual python file copy methods don't preserve all the metadata available on MacOS, export uses /usr/bin/ditto to do the copy for export. ditto preserves most metadata such as extended attributes, permissions, ACLs, etc.

### Utility Functions

The following functions are located in osxphotos.utils

#### ```get_system_library_path()```

**MacOS 10.15 Only** Returns path to System Photo Library as string.  On MacOS version < 10.15, raises Exception.

#### ```get_last_library_path()```

Returns path to last opened Photo Library as string.  

#### ```list_photo_libraries()```

Returns list of Photos libraries found on the system.  **Note**: On MacOS 10.15, this appears to list all libraries. On older systems, it may not find some libraries if they are not located in ~/Pictures.  Provided for convenience but do not rely on this to find all libraries on the system.

#### ```dd_to_dms_str(lat, lon)```
Convert latitude, longitude in degrees to degrees, minutes, seconds as string.
lat: latitude in degrees
lon: longitude in degrees
returns: string tuple in format ("51 deg 30' 12.86\\" N", "0 deg 7' 54.50\\" W")
This is the same format used by exiftool's json format.

#### ```create_path_by_date(dest, dt)```
Creates a path in dest folder in form dest/YYYY/MM/DD/
dest: valid path as str
dt: datetime.timetuple() object
Checks to see if path exists, if it does, do nothing and return path. If path does not exist, creates it and returns path.  Useful for exporting photos to a date-based folder structure.

### Examples

```python
import osxphotos

def main():
    photosdb = osxphotos.PhotosDB()
    print(f"db file = {photosdb.db_path}")
    print(f"db version = {photosdb.db_version}")

    print(photosdb.keywords)
    print(photosdb.persons)
    print(photosdb.albums)

    print(photosdb.keywords_as_dict)
    print(photosdb.persons_as_dict)
    print(photosdb.albums_as_dict)

    # find all photos with Keyword = Kids and containing person Katie
    photos = photosdb.photos(keywords=["Kids"], persons=["Katie"])
    print(f"found {len(photos)} photos")

    # find all photos that include Katie but do not contain the keyword wedding
    photos = [
        p
        for p in photosdb.photos(persons=["Katie"])
        if p not in photosdb.photos(keywords=["wedding"])
    ]

    # get all photos in the database
    photos = photosdb.photos()
    for p in photos:
        print(
            p.uuid,
            p.filename,
            p.date,
            p.description,
            p.title,
            p.keywords,
            p.albums,
            p.persons,
            p.path,
            p.ismissing,
            p.hasadjustments,
        )


if __name__ == "__main__":
    main()
```

## Related Projects

[photosmeta](https://github.com/rhettbull/photosmeta): uses osxphotos and [exiftool](https://exiftool.org/) to apply metadata from Photos as exif data in the photo files.  Can also export photos while preserving metadata and also apply Photos keywords as spotlight tags to make it easier to search for photos using spotlight.

## Contributing

Contributing is easy!  if you find bugs or want to suggest additional features/changes, please open an [issue](https://github.com/rhettbull/osxphotos/issues/).

I'll gladly consider pull requests for bug fixes or feature implementations.  

If you have an interesting example that shows usage of this module, submit an issue or pull request and i'll include it or link to it.

Testing against "real world" Photos libraries would be especially helpful.  If you discover issues in testing against your Photos libraries, please open an issue.  I've done extensive testing against my own Photos library but that's a since data point and I'm certain there are issues lurking in various edge cases I haven't discovered yet.

## Implementation Notes

This module works by creating a copy of the sqlite3 database that photos uses to store data about the photos library. the class photosdb then queries this database to extract information about the photos such as persons (faces identified in the photos), albums, keywords, etc.  If your library is large, the database can be hundreds of MB in size and the copy then read can take many 10s of seconds to complete.  Once copied, the entire database is processed and an in-memory data structure is created meaning all subsequent accesses of the PhotosDB object occur much more quickly. 

If apple changes the database format this will likely break.

Apple does provide a framework ([PhotoKit](https://developer.apple.com/documentation/photokit?language=objc)) for querying the user's Photos library and I attempted to create the funcationality in this module using this framework but unfortunately PhotoKit does not provide access to much of the needed metadata (such as Faces/Persons).  While copying the sqlite file is a bit kludgy, it allows osxphotos to provide access to all available metadata.

## Dependencies
- [PyObjC](https://pythonhosted.org/pyobjc/)
- [PyYAML](https://pypi.org/project/PyYAML/)
- [Click](https://pypi.org/project/click/)

## Acknowledgements
This project was originally inspired by photo-export by Patrick Fältström see: (https://github.com/patrikhson/photo-export) Copyright (c) 2015 Patrik Fältström paf@frobbit.se

