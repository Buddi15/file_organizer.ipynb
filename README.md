# file_organizer.ipynb
I created this Python script to automatically organize files in any folder by their types. It scans file extensions and moves files into Documents, Images, Videos, or Others folders, creating these folders if they don’t exist. This saves time and keeps my files neat and easy to find.
# file_organizer.py
import os
import shutil

FILE_TYPES = {
    "Documents": ['.pdf', '.doc', '.docx', '.txt', '.xls', '.xlsx', '.ppt', '.pptx'],
    "Images": ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.tiff', '.svg'],
    "Videos": ['.mp4', '.mov', '.avi', '.mkv', '.flv', '.wmv']
}

def organize_files(directory):
    if not os.path.exists(directory):
        print("The specified directory does not exist.")
        return

    for folder in FILE_TYPES.keys():
        os.makedirs(os.path.join(directory, folder), exist_ok=True)
    os.makedirs(os.path.join(directory, "Others"), exist_ok=True)

    for item in os.listdir(directory):
        item_path = os.path.join(directory, item)
        if os.path.isdir(item_path):
            continue

        _, ext = os.path.splitext(item)
        ext = ext.lower()

        moved = False
        for category, extensions in FILE_TYPES.items():
            if ext in extensions:
                shutil.move(item_path, os.path.join(directory, category, item))
                moved = True
                break

        if not moved:
            shutil.move(item_path, os.path.join(directory, "Others", item))

    print("Files have been organized successfully!")

if __name__ == "__main__":
    path = input("Enter the full path of the directory to organize: ").strip()
    organize_files(path)


