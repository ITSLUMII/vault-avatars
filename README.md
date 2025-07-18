# jf-avatars-images

This project automatically generates the JSON metadata files needed to use custom avatars with the [jf-avatars](https://github.com/kalibrado/jf-avatars) project.

A Python script, `image_indexer.py`, scans an image folder (including subfolders for categories) and creates:

* `images_metadata.json`: a list of images with their URLs, dimensions, size, etc.
* `folders_names.json`: a list of category names (the subfolder names).

---

## Prerequisites

* Python 3.x
* Pillow library: install via `pip install pillow`

---

## Installation & Usage

1. Clone this repository:

```bash
git clone https://github.com/kalibrado/jf-avatars-images.git
cd jf-avatars-images
```

2. Place your avatar images inside an `images/` folder. You can organize avatars by category using subfolders, for example:

```
images/
├── Steam/
├── Xbox One/
├── Pop Culture/
└── etc...
```

3. Edit the `image_indexer.py` file to match your environment:

```python
base_url = "http://192.168.X.X:8096/web/images/"
image_folder = "images"
output_src_images = "images_metadata.json"
output_src_folders = "folders_names.json"
```

* `base_url`: the URL Jellyfin uses to access your images folder.
* `image_folder`: local path to your images folder (default is `images/`).
* `output_src_images` & `output_src_folders`: names of the generated JSON files.

4. Place the `images/` folder in a location accessible by Jellyfin, typically the `web/` folder.

> If using Docker, mount a volume to `/usr/share/jellyfin/web/images/` containing your `images/` folder.

5. Run the script:

```bash
python3 image_indexer.py
```

6. Two JSON files will be generated in the current folder:

* `images_metadata.json`
* `folders_names.json`

Copy them to the same `web/` folder where `images/` is located.

---

## Jellyfin Configuration

To use your custom avatars, add the following lines in **Dashboard > Display > Custom CSS**:

```css
--jf-avatars-url-images: 'http://192.168.X.X:8096/web/images_metadata.json';
--jf-avatars-url-images-cat: 'http://192.168.X.X:8096/web/folders_names.json';
```

---

## FAQ

**Q: Can I use this script with offline images?**
A: Yes, as long as Jellyfin can access the images via a local URL (e.g., placed in the `web/` folder).

**Q: How do I organize avatars into categories?**
A: Simply create subfolders inside `images/`. The script automatically detects these folder names as categories.

**Q: I’m not using Docker; where should I place the files?**
A: Usually `/usr/share/jellyfin/web/` on Linux or `C:\Program Files\Jellyfin\Server\jellyfin-web` on Windows.
